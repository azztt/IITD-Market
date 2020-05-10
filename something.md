# API Implementations

Implementations for the various routes and their respective endpoints are described below:

---

### /user

#### POST /login

```
// login user
Algo:
```

#### POST /register

```
// register new user
Algo:
```

#### GET

```js
// get logged user
//Algo:
```

#### PUT

```
// update user profile
Algo:
```

#### DELETE

```
// delete user profile (sensitive operation)
Algo:
```

---

### /item

#### GET

```js
// get all items
Algo:
// Define escapeRegex function for search feature
function escapeRegex(text) {
    return text.replace(/[-[\]{}()*+?.,\\^$|#\s]/g, "\\$&");
};

    //INDEX - show all courses
router.get("/item", function(req, res){
  var perPage = 8;
  var pageQuery = parseInt(req.query.page);
  var pageNumber = pageQuery ? pageQuery : 1;
  var noMatch = null;
    if(req.query.search) {
      const regex = new RegExp(escapeRegex(req.query.search), 'gi');
      Item.find({title: regex}).skip((perPage * pageNumber) - perPage).limit(perPage).exec(function (err, allItems) {
        Item.count({name: regex}).exec(function (err, count) {
          if (err) {
            console.log(err);
            return res.redirect("back");
          } else {
            if(allItems.length < 1) {
              noMatch = "No Items match that query, please try again.";
            }
            res.status(200).json({
              items: ...allItems,
              current: pageNumber,
              pages: Math.ceil(count / perPage),
              noMatch: noMatch,
              search: req.query.search
            });
          }
        });
      });
    } else {
      // get all items from DB
      Item.find({}).skip((perPage * pageNumber) - perPage).limit(perPage).exec(function (err, allItems) {
        Item.count().exec(function (err, count) {
          if (err) {
            console.log(err);
          } else {
            res.status(200).json({
              items: allItems,
              current: pageNumber,
              pages: Math.ceil(count / perPage),
              noMatch: noMatch,
              search: false
            });
          }
        });
      });
    }
});

```

#### POST

```js
// post new item
Algo:
var multer = require('multer');
var storage = multer.diskStorage({
  filename: function(req, file, callback) {
    callback(null, Date.now() + file.originalname);
  }
});
var imageFilter = function (req, file, cb) {
    // accept image files only
    if (!file.originalname.match(/\.(jpg|jpeg|png|gif)$/i)) {
        return cb(new Error('Only image files are allowed!'), false);
    }
    cb(null, true);
};
var upload = multer({ storage: storage, fileFilter: imageFilter})
//CREATE - add new course to DB
router.post("/", upload.array('image'), function(req, res){
  // get data from form and add to courses array
  cloudinary.v2.uploader.upload(req.file.path,function(err,result){
    if(err) {
      req.flash('error', err.message);
      return res.redirect('back');
    }
  var title = req.body.title,
      image = result.secure_url,
    imageId = result.public_id,
       desc = req.body.description,
     seller = req.user.id,
   category = req.body.category,
        tag = req.body.tag,
      price = req.body.price,
 approxTime = {
		month: req.body.month,
		year: req.body.year
	};
  var newItem = {title: title, images: image, imageId:imageId, description: desc, price: price, seller:seller, category: category, tag: tag, approxTime: ...approxTime};
    // Create a new course and save to DB
    try {
      let item = await Item.create(newItem);
      let users = await User.find({folCategory:`${category}`}).exec();
      let newNotification = {
        targetUser: req.user.id,
        targetId: item._id,
        message: "created a new item"
      }
      for(const follower of user) {
        let notification = await Notification.create(newNotification);
        await follower.notifs.push(notification);
        follower.save();
      }
      //redirect back to courses page
      res.redirect(`/course/${course.slug}`);
    } catch(err) {
      req.flash('error', err.message);
      return res.redirect('back');
    }
});
});
```

#### PUT /:id

```js
// send item details
Algo:
router.put("/:id", upload.array('image'), function(req, res){
  Item.findById(req.params,id).exec(async function(err, item){
        if(err){
            res.status(333);
        } else {
        try{
          if(req.file){
            try{
              await cloudinary.v2.uploader.destroy(item.imageId);
              var result = await cloudinary.v2.uploader.upload(req.file.path); 
              course.imageId = result.public_id;
              course.image = result.secure_url; 
            } catch(err){
              return res.status(333);
            }
           }
           let user = await User.findOne({slug:course.author.slug}).populate('followers').exec();
           let newNotification = {
            username: req.user.username,
            targetSlug: course.slug,
            isCourse: true,
            message: "updated course: " + req.body.title
          }
          for(const follower of user.followers) {
            let notification = await Notification.create(newNotification);
            await follower.notifications.push(notification);
            follower.save();
          }
          course.title = req.body.title;
          course.description = req.body.description;
          course.studentNo = req.body.studentNo;
           course.location = location;
           course.lat = lat;
           course.lng = lng;
           await course.save();
            req.flash("success","Successfully Updated!");
            res.redirect("/course/" + course.slug);
          } catch(err){
            console.log(err)
            req.flash("error", err.message);
            return res.redirect("back");
          }
        }
    });
});

```

#### GET /:id

```js
// edit item details (includes any edit operation update, sold, etc)
Algo:
router.get("/:id", function(req, res){
    //find the course with provided ID
  Item.findById(req.params.id).exec(function(err, foundItem){
    if(err || !foundItem){
      console.log(err);
      req.status(333);
    }
    //render show template with that course
    res.status(200).json(foundItem);
  });
});

```

#### DELETE /:id

```js
// post new item
Algo:
// DELETE - removes course and its comments from the database
router.delete("/:id",async function(req, res) {
	let item = await Item.findById(req.params.id).populate('chat').exec();
	let newNotification = {
      targetUser: req.user.id,
      targetId: item._id,
      message: "deleted an item"
	}
	for (let notifusers of item.chats) {
		let notification = await Notification.create(newNotification);
		let userx = await User.findById(notifusers.user2).exec();
    await userx.notifications.push(notification);
		await userx.save();
		Chat.findOneAndRemove({_id: notifusers.id})
	}
	await item.remove();
	res.status(206).send('Deleted');
})
```

---

### /review

#### POST

```
// post review for a user
Algo:
```

#### PUT /:id

```
// edit any review
```

#### DELETE

```
// delete any review
Algo:
```

---

### /notification

---

// figure out how to manage chats in db after any chat session
