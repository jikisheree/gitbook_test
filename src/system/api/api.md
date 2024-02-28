# API Design


## Post/Event Ranking 

\\[ \text{score}(\text{post}) = \frac{\sqrt(1+\text{post.likes})}{1+\text{post.likes}} \times
    \frac{\alpha\times\log(1+\text{post.likes})}{1+\log(1+\beta\times(\text{time.now}-\text{post.time}))} 
\\]

The above equation defines how we score each post, \\(\alpha\\) is how much we care about the likes and 
\\(\beta\\) is how much we care about how recent the post is. These 2 values can be adjusted to suit 
our needs. Below is an example of how the equation work.

|Likes|Time|\\(\alpha\\)|\\(\beta\\)|Score|
|--:|--:|--:|--:|--:|
|0|0|2|0.85|1|
|1|5|2|0.85|1.057109|
|100|3600|2|0.85|0.99312|
|10|3600|2|0.85|0.765811|
|200|1800|2|0.85|1.171232|

The post with higher score will be recommended to user.

## Group Ranking 

\\[( \text{score}(\text{group}) = \alpha\times\text{group.members}) 
\times (\beta\times\text{group.recent\_acts})
\\]

The above equation defines how we score each group, \\(\alpha\\) is how much we care about the 
amount of members in a group and 
\\(\beta\\) is how much we care about how much recent activites a group has. 
These 2 values can also be adjusted to suit our needs. 
Below is an example of how the equation work.

|Members|Recent Activities|\\(\alpha\\)|\\(\beta\\)|Score|
|--:|--:|--:|--:|--:|
|10|20|1|1.15|13|
|1000|10|1|1.15|1015|
|500|200|1|1.15|800|
|900|100|1|1.15|1050|

The group with higher score will be recommended to user.

## Application API
We will use REST API for application's side API. In the following section will be the 
draft API endpoint with a data schema if needed.
```typescript
interface ExampleSchema {
    a: string   // required field 'a' with type 'string'
    b?: number  // optional field 'b' with type 'number'
}
```

### Social API
#### Retrieving data
`GET /api/self` will return the user info of the caller (`User`)

`GET /api/users/{communityId}` will return a list of users in a community
```typescript
interface User {
    firstname: string
    lastname: string
    nickname: string
    gender: string
    location: Location
    profilePicture: string // URL of the image
}

inteface Users {
    data: User[]
    pagination: {
        next?: string
        prev?: string
    }
}
```

`GET /api/explore` will return a recommended paginated list of communities

`GET /api/communities` will return a paginated list of communities
```typescript
interface Community {
    thumbnail: string // URL of the image
    name: string
    description: string
    members: number
    likes: number
    posts: number
}

interface Communities {
    data: Community[]
    pagination: {
        next?: string
        prev?: string
    }
}
```

`GET /api/posts` will return a paginated list of posts 
- Use this on both global feed and community feed
- Event is also a post, we can get only the events by specify `isEvent=true`

```typescript
interface Comment {
    owner: string,
    content: string
}

interface EventPost {
    owner: string
    images?: string[] // URL of the image 
    header: string
    description: string
    joined: number
    datetime: DateTime
    shared: number
    comments: Comment[]
}

interface Post {
    owner: string
    images?: string[] // URL of the image 
    header: string
    description: string
    likes: number
    shared: number
    comments: Comment[]
}

interface Posts {
    data: Post[] | EventPost[]
    pagination: {
        next?: string
        prev?: string
    }
}
```

`GET /api/user` will return the user information
```typescript
interface User {
    profileImage: string // URL of the image
    name: string
    role: string
}
```

#### Creating data
`POST /api/signup` for creating a new user
```typescript
interface NewUser {
    username: string
    phone: string
    birhday: Date
    email: string
    password: string
    confirmPassword: string
}
```


`POST /api/communities` for creating a new community
```typescript
interface NewCommunity {
    thumbnail: string // URL of the image
    name: string
    description: string
}
```

`POST /api/posts` for creating a new post
```typescript
interface NewPost {
    images?: string[] // URL of the image 
    header: string
    description: string
    isEvent: boolean
    datetime?: DateTime // only use this if isEvent is true
}
```

#### Mutating data
`PUT /api/self` for updating the user information
```typescript
interface UpdateUser {
    firstname: string
    lastname: string
    nickname: string
    gender: string
    location: Location
}
```

`PUT /api/self/pic` for updating the user profile picture
```typescript
interface UpdateUserProfilePic {
    profilePicture: string // URL of the image
}
```

`DELETE /api/communities/{id}` for deleting a community

`DELETE /api/posts/{id}` for deleting a post

`PUT /api/communities/{id}` for editing a community

```typescript
interface UpdateCommunity {
    thumbnail?: string // URL of the image
    name?: string
    description?: string
}
```
`PUT /api/posts/{id}` for editing a post

```typescript
interface UpdatePost {
    images?: string[] // URL of the image 
    header?: string
    description?: string
    isEvent?: boolean
    datetime?: DateTime // only use this if isEvent is true
}
```

`POST /api/posts/{id}/like` for liking a post

`POST /api/posts/{id}/share` for sharing a post to their own feed

`POST /api/posts/{id}/comment` for commenting a post

```
interface NewComment {
    owner: string,
    content: string
}
```


### Pet API

#### Retrieving data
`GET /api/pets` will return a list of pets of a certain user
```typescript
interface Pet {
    image: string
    breed: string
    color: string
    dateOfBirth: Date
    gender: 'Male' | 'Female'
    weight: number
    isSterilized: boolean
}

interface Pets {
    data: Pet[]
    pagination: {
        next?: string
        prev?: string
    }
}
```

`GET /api/pets/location` will return a gps location of the pet (both lost and not lost)
```typescript
interface PetLocation {
    current: GPS
    history: GPS[]
}
```

`GET /api/pets/health` will return the health information of certain pet
```typescript
interface PetHealth {
    heartrate: number
    calories: number
    rest: number
    healthrecords: {
        datetime: DateTime
        content: string
    }[]
}
```

`GET /api/pets/health/mood` will return mood histort of the pet
```typescript
interface PetMood {
    data: { datetime: DateTime, value: MoodValue }[]
    summary: [MoodValue, number][]
}
```

`GET /api/pets/health/heartrate` will return heart rate history of the pet
```typescript
interface PetHeartRate {
    range: [number, number]
    restingHeartRate: number
    data: { datetime: DateTime, value: number }[]
}
```

`GET /api/pets/health/calories` will return calories history of the pet
```typescript
interface PetCalories {
    data: { datetime: DateTime, value: number }[]
}
```

`GET /api/pets/health/rest` will return reset history of the pet
```typescript
interface PetRest {
    data: { datetime: DateTime, value: number }[]
}
```

`GET /api/pets/vaccines` will return a list of vaccines that a pet has
```typescript
interface Vaccine {
    date: Date
    description: string
}

interface Vaccines {
    data: Vaccine[]
    pagination: {
        next?: string
        prev?: string
    }
}
```

#### Creating data
`POST /api/pets` for adding a new pet
```typescript
interface NewPet {
    image: string
    breed: string
    color: string
    dateOfBirth: Date
    gender: 'Male' | 'Female'
    weight: number
    isSterilized: boolean
}
```

`POST /api/pets/health/record/{petname}` for adding a new health record to certain pet
```typescript
interface NewPetHealthRecord {
    datetime: DateTime
    content: string
}
```

`POST /api/pets/vaccines/{petname}` for adding a new vacccine to certain pet
```typescript
interface NewVaccine {
    date: Date
    description: string
}
```

#### Mutating data
`PUT /api/pets/{petname}` for editing a pet information
```typescript
interface UpdatePet {
    image?: string
    breed?: string
    color?: string
    dateOfBirth?: Date
    gender?: 'Male' | 'Female'
    weight?: number
    isSterilized?: boolean
}
```

`PUT /api/pets/health/record/{petname}/{id}` for editing a certain pet's health record
```typescript
interface UpdatePetHealthRecord {
    datetime?: DateTime
    content?: string
}
```

`PUT /api/pets/vaccines/{petname}` for editing a certain pet's vaccine
```typescript
interface UpdateVaccine {
    date?: Date
    description?: string
}
```

### Collar API 

> MQTT, or Message Queuing Telemetry Transport, is a lightweight and open-source messaging protocol 
> designed for efficient communication between devices in a distributed and resource-constrained network. 
> It follows a publish-subscribe model, allowing devices to publish messages to specific topics, and other devices
> to subscribe to those topics to receive relevant information. MQTT is known for its simplicity, 
> low bandwidth usage, and support for unreliable or intermittent networks. It is widely used in 
> Internet of Things (IoT) applications, where devices need to exchange data in a scalable 
> and reliable manner. MQTT's design makes it suitable for scenarios where low latency and 
> minimal network overhead are essential, making it a popular choice for various IoT implementations.


We will use "Fan-In" pattern as stated on this
[article](https://docs.aws.amazon.com/whitepapers/latest/designing-mqtt-topics-aws-iot-core/mqtt-communication-patterns.html#fan-in)
from AWS.

![fan-in](../../images/many-to-one-fan-in.png)

Collar will have this topic schema `collar/{cat | dog}/{deviceId}|{gps | sound | heartrate}` 
e.g. `collar/cat/fed38152-6595-48c1-aaea-ebc0d937a19d/{gps}` and the payload will look like this

```  
// For gps, every 5min send gps location with timestamp
{
    lat,
    long,
    timestamp,
}[]
```
```
// For sound, send sound recorded (in byte array format) in certain period
{
    70 db c9 dc f4 2a 76 dc 46 47 6c fd e2 5c a6 ea 
    f7 85 4f b7 59 aa b4 47 b3 ea 97 74 1d 23 f6 5e 
    d7 43 c5 84 8a 4b 66 ba 46 95 fc d1 64 47 82 77 
    2c 57 12 73 98 cf 07 57 b7 02 5e c1 aa 31 1f 23 
    96 19 bf 23 cc 4f fa 41 e1 78 4d 0a 82 31 29 76 
    18 43 b7 68 e7 11 52 e8 e1 8b 38 70 5a 71 ff 61 
    3a 7e 5f e5 b5 23 87 80 7a 7c 81 48 88 36 36 db 
    57 67 22 bd 4e c3 29 34 db 79 6a 4c c1 65 1d dc 
    39 38 3a c4 db b1 e9 d4 ec 87 71 18 e1 68 fb 9e 
    a4 59 04 4e c9 30 a9 ac f6 eb 36 52 f1 4a e5 df 
    9b d6 08 9b 06 cc 8a 53 de fc ab f5 b0 53 7c 22 
    7d f3 c8 8b 8f 92 04 43 36 cb 60 45 e6 d8 09 bd 
    b7 6e 35 37 35 21 a6 0f ...
}
```

```
// For heartrate, every 10s send heartrate with timestamp
{
    bpm,
    timestamp
}[]

```
Then we will publish this to the broker using QOS 1 (at least once).
