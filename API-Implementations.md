# API Implementations

Implementations for the various routes and their respective endpoints are described below:

Some common res
```js
res: {
	locals: {
		moment: require('moment')
	}
}
app.use(async function(req, res, next){
   res.locals.currentUser = req.user;
   if(req.user) {
    try {
      let user = await User.findById(req.user._id).populate('notifs', null, { isRead: false }).exec();
      res.locals.notifications = user.notifs.reverse();
    } catch(err) {
      console.log(err.message);
    }
   }
   res.locals.success = req.flash('success');
   res.locals.error = req.flash('error');
   next();
});
```

---

### /user

#### POST /login

```js
// login user
Algo:

```

#### POST /register

```js
// register new user
Algo:
```

#### GET /:id

```js
// get logged user
Algo:
res: {
	user: UserObject <UserSchema>
}
```

#### PUT /:id
Basic Steps:
1. Check for auth
2. Find the user
3. Update the fields
4. Save it
```js
// update user profile
Algo:
req: {
	body: {
		should contain new data
	}
	files: {
		should contain files
	}
}
```

#### DELETE /:id
Basic Steps:
1. Check for auth
2. Delete it
```js
// delete user profile (sensitive operation)
Algo:
```

#### GET /:id/notification
Basic Steps:
1. Check for auth
2. Find the user
3. Populate all notification of user
4. Send all the notifs data
```js
Algo: 
res: {
	Notifs: [...notification]
}
```

---

### /item

#### GET

Basic Steps: 
1. Search for req items if search specified in query else send all items
```js
// get all items
Algo:
req: {
	query: {
		page: Number,
		search: String
	}
res: {
	items: ItemsObject,
  current: Number,
  pages: Math.ceil(count / perPage),
  noMatch: String,
  search: String
}
}
```

#### POST
Basic Steps:
1. Check for auth
2. Calls create item fn for new item
3. Notify all the user who has wishlist for item in particular category
```js
// post new item
Algo:
req: {
	body: {
		body of Item
	}
	files: {
		contains the files
	}
}
res: {
	message: String
}
```

#### PUT /:id
Basic Steps:
1. Check for auth
2. Find the item
3. Update the item
4. Notify all the users that have chatted about this item
```js
// edit item details (includes any edit operation update, sold, etc)
Algo:
req: {
	body: {
		body of Item
	}
	files: {
		contains the files
	}
}
res: {
	message: String
}
```

#### GET /:id
Basic Steps:
1. Find the item
2. If the user === seller then send all chats related to the item
3. Else if the chats.user2 === current user then send only that chat
```js
// send item details
Algo:
res: {
	item: Founditem,
	chats: [chatObject] 
}
```

#### DELETE /:id
Basic Steps:
1. Check for auth
2. Send notif to all the users who are chatting about this item
3. Deleting the item
```js
// post new item
Algo:
```

#### POST /:id/sellIni
Basic Steps:
1. Check for auth
2. Find the item
3. Check for validity of buyer
4. Change the status of item to 'INPROCESS'
5. Send the confirmation notice to buyer and freeze all the chats regarding the item
```js
// Initialize selling procedure
Algo:
req: {
	body: {
		email: String,
		id: String
	}
}
```

#### POST /:id/sellFin
Basic Steps:
1. Check for auth
2. Find the item
3. If he/she accepts, Change the status of item to 'SOLD'
4. If he/she rejects, Change the status to 'UNSOLD' and send the notice to seller about rejection and unfreeze the chat
```js
// Initialize selling procedure
Algo:
req: {
	body: {
		accept: Boolean,
		id: String
	}
}
```

#### POST /:id/report
Basic Steps:
1. Check for auth
2. Find the item
3. Report it
4. Save the item
```js
// Report the item
Algo:
```

#### POST /:id/chat/new
Basic Steps:
1. Check for auth
2. Find the item
3. Create a chat obj and start with initial message
4. Push chat ref to item
5. Save the item
```js
// Starts the new chat
req: {
	body: {
		user1: id,
		user2: id,
		item: id
	}
}
Algo:
```

#### GET /:id/chat/:idChat
Basic Steps:
1. Check for auth
2. Find the chat
3. Check for valid user
4. Send the chat
```js
// Report the item
Algo:
res: {
	chat: chat
}
```

#### PUT /:id/chat/:idChat
Basic Steps:
1. Check for auth
2. Find the chat
3. Check for valid user
4. Push the message in chat
5. Save the chat
```js
// Sends new message
req: {
	body: {
		message:message
	}
}
Algo:
```

---

### /review

#### POST
Basic Steps:
1. Check for auth
2. Find the user associated
3. Create a new review
4. Notify the user about the review
```js
// post review for a user
Algo:
req: {
	body: Content of review
}
```

#### PUT /:id
Basic Steps:
1. Check for auth
2. Find the user associated
3. Update the review
4. Notify the user about the review
```js
// edit any review
Algo:
req: {
	body: new Content of review
}
```

#### DELETE /:id
Basic Steps:
1. Check for auth
2. Find the user associated
3. Delete the review
4. Notify the user about the review
```js
// delete any review
Algo:
```

#### POST /:id/upvote
Basic Steps:
1. Check for auth
2. Find the review
3. Upvote it
4. Save the review
```js
// Upvotes the review
Algo:
res: {
	vote: Current votes
}
```

#### POST /:id/downvote
Basic Steps:
1. Check for auth
2. Find the review
3. Doownvote it
4. Save the review
```js
// Downvotes the review
Algo:
res: {
	vote: Current votes
}
```

#### POST /:id/report
Basic Steps:
1. Check for auth
2. Find the review
3. Report it
4. Save the review
```js
// Report the review
Algo:
```

---

// figure out how to manage chats in db after any chat session
