# API Design


### Application Requirements (REST)
- Social
    - Community (GET, POST, DELETE, PUT)
    - Post (GET, POST, DELETE, PUT)
- Personal
    - User Info
    - Pet Info (including GPS loc)
    - Pet Health

### Social API
#### Retrieving data
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
interface EventPost {
    owner: string
    images?: string[] // URL of the image 
    header: string
    description: string
    joined: number
    datetime: DateTime
}

interface Post {
    owner: string
    images?: string[] // URL of the image 
    header: string
    description: string
    likes: number
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
`DELETE /api/communities/{id}` for deleting a community

`DELETE /api/posts/{id}` for deleting a post

`PUT /api/communities/{id}` for editing a community

`PUT /api/posts/{id}` for editing a post

### Pet API
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


### Collar Requirements (MQTT)
- Sending data 
    ```
    {
       gps
       sound
       heartrate
    }
    ```

What we need 
- MQTT Broker 
- MQTT Client on the collar that will publish the data to the broker
- Send `at least once`





