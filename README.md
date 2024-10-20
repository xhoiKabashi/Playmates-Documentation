# Pet Matchmaking Application Documentation

## Use Cases

### 1. User accessing the website / User Authentication
- **As a new user:**
  - Register using email/password
  - Register using Google/Facebook (optional)
  - Verify email address
  - User clicks the "Sign Up" button.
  - Create initial profile
  - System redirects the user to the Dashboard .
- **As a registered user:**
  - Log in using email/password
  - Log in using Google/Facebook (optional)
  - Reset forgotten password
  - User clicks the "Login" button.
  - System verifies the credentials.
  - System redirects the user to the Dashboard .
### 2. Dashboard 
- **As a user  already logged:**
  - User can acess Pet Profile Management Page
  - Use can access Matchmaking Page
  - User can acces Messages
  - User can acess Blog/Community Page
  - User can Acess Marketplace Page
  - User can acces Adoption Center Page
  
### 3. Profile Management
- **As a pet owner:**
  - Create pet profile
  - Upload pet photos
  - Edit pet information
  - Update availability status
  - Manage profile visibility
  - Set location preferences
  - Update personal information
### 4. Pet Matchmaking
- **As a pet owner seeking playmates:**
  - Set search criteria
  - View potential matches
  - Filter matches by location/species/age
  - Send match requests
  - Accept/reject incoming requests
  - Schedule playdates
  - Rate past interactions

### 5. Communication
- **As a matched user:**
  - Send direct messages
  - Share photos in chat
  - Block/report users
  - Set chat preferences
  - Schedule meetups
  - Share location for meetups

### 6. Content Management
- **As a community member:**
  - Create blog posts
  - Comment on posts
  - Share experiences
  - Upload photos/videos
  - Rate and review services
  - Report inappropriate content

### 7. Adoption Platform
- **As a shelter/individual:**
  - List pets for adoption
  - Upload pet documentation
  - Manage adoption listings
  - Process adoption requests
- **As an adopter:**
  - Browse available pets
  - Filter adoption listings
  - Contact pet owners/shelters
  - Save favorite listings

### 8. Marketplace Interaction
- **As a seller:**
  - List products/services
  - Manage inventory
  - Process orders
  - Respond to inquiries
- **As a buyer:**
  - Browse products/services
  - Place orders
  - Leave reviews
  - Track order status

## General Application Flow

```mermaid
graph TD
    A[Landing Page] --> B{User Status}
    B -->|New User| C[Registration]
    B -->|Existing User| D[Login]
    
    C --> E[Profile Creation]
    E --> F[Dashboard]
    D --> F[Dashboard]
    
    F --> G[Pet Profile Management]
    F --> H[Matchmaking]
    F --> I[Messages]
    F --> J[Blog/Community]
    F --> K[Marketplace]
    F --> L[Adoption Center]
    F --> S[Settings]


    
    H --> M{Match Found?}
    M -->|Yes| N[Chat Interface]
    M -->|No| O[Continue Searching]
    
    N --> P[Schedule Playdate]
    P --> Q[Rating/Review]
```

## Technical Implementation Details

### Frontend Structure
```
/public
  ├── index.html
  ├── css/
  │   ├── style.css
  │   ├── components/
  │   └── pages/
  ├── js/
  │   ├── app.js
  │   ├── router.js
  │   ├── components/
  │   └── services/
  └── assets/
      ├── images/
      └── icons/
```

### Backend Structure
```
/server
  ├── server.js
  ├── config/
  ├── routes/
  ├── controllers/
  ├── models/
  ├── middleware/
  └── utils/
```

### API Endpoints

1. Authentication
```
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
PUT /api/auth/reset-password
```

2. Profile Management
```
GET /api/profile/:userId
PUT /api/profile/:userId
POST /api/profile/pet
PUT /api/profile/pet/:petId
```

3. Matchmaking
```
GET /api/matches
POST /api/matches/request
PUT /api/matches/:matchId
GET /api/matches/filter
```

4. Communication
```
GET /api/messages
POST /api/messages
GET /api/messages/:chatId
```

5. Content
```
GET /api/posts
POST /api/posts
PUT /api/posts/:postId
DELETE /api/posts/:postId
```

### Security Considerations
- JWT for authentication
- Input validation
- XSS protection
- CORS configuration
- Rate limiting
- Data encryption
- Secure file upload handling

### Performance Optimization
- Image compression
- Lazy loading
- Caching strategies
- Pagination
- Database indexing
- Asset minification
erDiagram
    USERS {
        int UserID PK
        string Email
        string PasswordHash
        boolean IsEmailVerified
        string ResetPasswordToken
        datetime ResetPasswordExpires
        datetime CreatedAt
        datetime UpdatedAt
    }
    OAUTHPROVIDERS {
        int OAuthID PK
        int UserID FK
        string ProviderName
    }
    USERPROFILES {
        int ProfileID PK
        int UserID FK
        string FirstName
        string LastName
        string Location
        string ProfilePicture
    }
    PETPROFILES {
        int PetID PK
        int UserID FK
        string PetName
        string Species
        int Age
        string Photo
        string Status
    }
    MATCHES {
        int MatchID PK
        int PetID FK
        int MatchedPetID FK
        string MatchStatus
    }
    PLAYDATES {
        int PlaydateID PK
        int MatchID FK
        datetime ScheduledAt
        string Location
    }
    MESSAGES {
        int MessageID PK
        int SenderUserID FK
        int ReceiverUserID FK
        string Content
        datetime SentAt
    }
    POSTS {
        int PostID PK
        int UserID FK
        string Title
        string Content
        datetime CreatedAt
    }
    COMMENTS {
        int CommentID PK
        int PostID FK
        int UserID FK
        string Content
        datetime CommentedAt
    }
    ADOPTIONLISTINGS {
        int ListingID PK
        int PetID FK
        string AdoptionStatus
        datetime ListedAt
    }
    ADOPTIONREQUESTS {
        int RequestID PK
        int UserID FK
        int ListingID FK
        string RequestStatus
        datetime RequestedAt
    }
    PRODUCTLISTINGS {
        int ProductID PK
        int UserID FK
        string ProductTitle
        float Price
        int Stock
    }
    ORDERS {
        int OrderID PK
        int ProductID FK
        int UserID FK
        int Quantity
        float TotalPrice
        datetime OrderedAt
    }

    USERS ||--o{ OAUTHPROVIDERS : "has"
    USERS ||--o{ USERPROFILES : "has"
    USERS ||--o{ MESSAGES : "sends"
    USERS ||--o{ ADOPTIONREQUESTS : "makes"
    USERPROFILES ||--o{ PETPROFILES : "owns"
    PETPROFILES ||--o{ MATCHES : "participates in"
    MATCHES ||--o{ PLAYDATES : "schedules"
    POSTS ||--o{ COMMENTS : "has"
    PETPROFILES ||--o{ ADOPTIONLISTINGS : "listed in"
    ADOPTIONLISTINGS ||--o{ ADOPTIONREQUESTS : "receives"
    PRODUCTLISTINGS ||--o{ ORDERS : "sold in"
    USERS ||--o{ ORDERS : "makes"

