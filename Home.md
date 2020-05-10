# Welcome to the IITD-Market wiki!

## Description
IITD Market is supposed to be a web/mobile based platform for the IITD community to sell/buy/rent products from one another inside the campus.
This wiki is intended to provide and maintain all the information about the Backend implementation details. This application is being built on the MERN (MongoDB, ExpressJS, ReactJS, NodeJS) stack.
The proposed documents in the mongodb database are:
1. User
2. Item
3. Review
4. Notification
5. Chat

The proposed schemas for each of the above documents are as follows:

### User
```coffee
{
    name: String,
    password: String,
    email: String,
    contact_number: String,
    entry_number: String,
    hostel: String,
    avatar: String,
    chats: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'Chat'
        }
    ],
    notifs: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'Notification'
        }
    ],
    review: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'Review'
        }
    ],
    folCategory: [
        {
            type: 'String
        }
    ]
}
```

### Item
```coffee
{
    title: String,
    seller: {
         type: mongoose.Schema.Types.ObjectId,
         ref: 'User'
    },
    buyer: {
         type: mongoose.Schema.Types.ObjectId,
         ref: 'User'
    },
    chats: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'Chat',
        }
    ],
    category: {
        type: String,
        enum: ["GENERAL","COOLER","LAPTOP","CYCLE","MATTRESS",],
        trim: true,
        default: "GENERAL",
    }
    tag: String,
    price: Number,
    description: String,
    images: [
        {
            type: String,
        }
    ],
    buy_date: Date,
    ApproxTime: {
        month: {
            type: Number,
            min: 1,
            max: 12,
        },
        year: Number,
    },
    status: {
        type: String,
        enum: ["UNSOLD","INPROCESS","SOLD"],
        default: 'UNSOLD'
    },
    isReported: {
        type: Boolean,
        default: false,
    },
    userIsAnonymous: {
        type: Boolean,
        default: false,
    },
}
```

### Review
```coffee
{
    rating: {
        type: Number,
        required: "Please provide a rating (1-5 stars).",
        min: 1,
        max: 5,
        validate: {
            validator: Number.isInteger,
            message: "{VALUE} is not an integer value."
        }
    },
    text: String,
    author: {
        type:  mongoose.Schema.Types.ObjectId,
        ref: "User"
    },
    // user associated with the review
    user: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "User"
    },
    isReported:{
        type:Boolean,
        default:false
    },
    isAnonymous:{
        type:Boolean, 
        default:false
    },
}
```

### Notification
```coffee
{
    targetUser: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "User"
    },
    message: String,
    isRead: {
        type: Boolean,
        default: false
    }
}
```

### Chat
```coffee
{
    user1: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "User"
    }
    user2: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "User"
    }
    item: {
        type: mongoose.Schema.Types.ObjectId,
        ref: "Item"
    }
    messages: [
         type: String
    ]
}
```

For various API endpoints and their implementation details see [API Implementations](https://github.com/azztt/IITD-Market/wiki/API-Implementations)
