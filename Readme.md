
# Features
- User Authentication: Users can register and log in securely using their credentials. We utilize Passport for authentication and store user data in MongoDB.

- Profile Management: Users can change their profile picture and update their information.

- Posting: Users can create posts similar to other social media platforms. They can upload images or files using Multer for image/file upload.

- Feed: Users can view posts from other users on their feed.

- Post Interaction: Users can like, comment, and save posts.

# Setup Instructions
- To set up the development environment and run the application locally, follow these steps:

- Install Dependencies: Run npm install to install all required dependencies.

- Database Configuration: Ensure you have MongoDB installed and running locally. Update the MongoDB connection details in your application configuration.

- Passport Configuration: Passport is used for authentication. Make sure to configure it properly, including setting up passport-local and passport-local-mongoose for local authentication strategy.

- File Upload Configuration: If you plan to allow users to upload images or files, configure Multer for handling file uploads.

- Generate Unique IDs: If needed, utilize UUID to generate unique IDs for posts or other entities.

- Run the Application: After completing the configuration, run npm start to start the server.

# API Endpoints
- /register: User registration endpoint.
- /login: User login endpoint.
- /logout: Endpoint to log out the user.
- /profile: Endpoint to view and update user profile.
- /feed: Endpoint to view posts from other users.
- /save/:pinId: Endpoint to save a post to a board.
- /delete/:pinId: Endpoint to delete a post from a board.
- /edit: Endpoint to edit a post.
- /upload: Endpoint for uploading images or files.

# Technologies Used
- Node.js: Runtime environment for the server.
- Express.js: Web application framework for Node.js.
- MongoDB: NoSQL database for storing user data and posts.
- Passport: Authentication middleware for Node.js.
- Multer: Middleware for handling file uploads.
- UUID: Library for generating unique IDs.
# Contributing
-We welcome contributions from the community! Feel free to submit bug reports, feature requests, or even pull requests to improve the application

# Express Router Configuration

This module sets up the routes for a social media platform using Express.js.

## Dependencies

- [express](https://www.npmjs.com/package/express): Fast, unopinionated, minimalist web framework for Node.js.
- [passport](https://www.npmjs.com/package/passport): Simple, unobtrusive authentication middleware for Node.js.
- [passport-local](https://www.npmjs.com/package/passport-local): Passport strategy for authenticating with a username and password.
- [multer](https://www.npmjs.com/package/multer): Middleware for handling `multipart/form-data`, used for file uploads.
- [users](./users.js): User model for MongoDB.
- [post](./post.js): Post model for MongoDB.

## Passport Configuration

Passport is configured with a local strategy using the `passport-local` module.

```javascript
passport.use(new localStrategy(userModel.authenticate()));



/ -login and register Screen
/logout
/register
/profile - profile page with boards
/feed - feed page with all different pins
/save/:pinId - pin will save a board 
/delete/:pinId - pin will delete to a board
/login
/edit
/upload



// Install dev.
npm i mongoose passport passport-local passport-local-mongoose express-session 
(multer for image or file upload) (uuid for uniqe id) 

[Functions]
User Can register and login
user can change her profile pic 
user can add a post like any social media 
user can see other users post on feed
user can view single post 

for login authentication i am use here passport 
connect on mongodb local use data will store 

/* express-router-configuration.js */

// Dependencies
var express = require('express');
var router = express.Router();
const userModel = require("./users");
const postModel = require("./post");
const passport = require('passport');
const localStrategy = require("passport-local");
const upload = require("./multer");

passport.use(new localStrategy(userModel.authenticate()));

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index',{nav: false});
});

router.get('/register', function(req, res, next) {
  res.render('register',{nav: false});
});

router.get('/profile', isLoggedIn,async function(req, res, next) {
  const user = await userModel
    .findOne({username: req.session.passport.user}) 
    .populate("posts");
  res.render('profile', {user, nav: true});
});

// add new post 
router.get('/add', isLoggedIn,async function(req, res, next) {
  const user = await userModel.findOne({username: req.session.passport.user});
  res.render('add', {user, nav: true});
});

// Create Post
router.post('/createpost', isLoggedIn, upload.single("postimage") ,async function(req, res, next) {
  const user = await userModel.findOne({username: req.session.passport.user});
  const post = await postModel.create({
    user: user._id,
    title: req.body.title,
    description: req.body.description,
    image: req.file.filename
  });
  user.posts.push(post._id);
  await user.save();
  res.redirect("/profile");
});

// show user all post 
router.get('/show/posts', isLoggedIn, async function(req, res, next){
  const user = await userModel
    .findOne({username: req.session.passport.user})
    .populate("posts");
  res.render('show', {user, nav: true});
});

// Show All user All Post On feed
router.get("/feed", isLoggedIn, async function(req, res, next){
  const user = await userModel.findOne({username: req.session.passport.user})
  const posts = await postModel.find().populate("user")
  res.render("feed", {user, posts, nav: true});
});

//show single post with ID
router.get("/feed/:id", isLoggedIn, async function(req, res, next){
  const user = await userModel.findOne({username: req.session.passport.user})
  const posts = await postModel.findOne({_id: req.params.id})
  console.log(posts)
  res.render("singlepost",{user, posts, nav: true});
});

// File Upload Route 
router.post('/fileupload', isLoggedIn, upload.single("image"),async function(req, res, next){
  const user = await userModel.findOne({username: req.session.passport.user})
  user.profileImage = req.file.filename;
  await user.save();
  res.redirect("/profile");
});

//Register Route
router.post('/register', function(req, res){
  const data = new userModel({
    username : req.body.username,
    email: req.body.email,
    contact: req.body.contact
  })
  userModel.register(data, req.body.password)
  .then(function(){
    passport.authenticate("local")(req, res, function(){
      res.redirect("/profile");
    })
  })
})

router.post('/login', passport.authenticate("local",{
  failureRedirect: "/",
  successRedirect: "/profile"
}),function(req, res, next) {
  res.render('profile');
});

router.get("/logout", function(req ,res, next){
 req.logout(function(err){
  if(err){return next(err);}
  res.redirect('/');
 });
})

function isLoggedIn(req, res, next){
  if(req.isAuthenticated()){
    return next();
  }
  res.redirect("/");
}

module.exports = router;
