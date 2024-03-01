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

Features
User Authentication: Users can register and log in securely using their credentials. We utilize Passport for authentication and store user data in MongoDB.

Profile Management: Users can change their profile picture and update their information.

Posting: Users can create posts similar to other social media platforms. They can upload images or files using Multer for image/file upload.

Feed: Users can view posts from other users on their feed.

Post Interaction: Users can like, comment, and save posts.

Setup Instructions
To set up the development environment and run the application locally, follow these steps:

Install Dependencies: Run npm install to install all required dependencies.

Database Configuration: Ensure you have MongoDB installed and running locally. Update the MongoDB connection details in your application configuration.

Passport Configuration: Passport is used for authentication. Make sure to configure it properly, including setting up passport-local and passport-local-mongoose for local authentication strategy.

File Upload Configuration: If you plan to allow users to upload images or files, configure Multer for handling file uploads.

Generate Unique IDs: If needed, utilize UUID to generate unique IDs for posts or other entities.

Run the Application: After completing the configuration, run npm start to start the server.

API Endpoints
/register: User registration endpoint.
/login: User login endpoint.
/logout: Endpoint to log out the user.
/profile: Endpoint to view and update user profile.
/feed: Endpoint to view posts from other users.
/save/:pinId: Endpoint to save a post to a board.
/delete/:pinId: Endpoint to delete a post from a board.
/edit: Endpoint to edit a post.
/upload: Endpoint for uploading images or files.
Technologies Used
Node.js: Runtime environment for the server.
Express.js: Web application framework for Node.js.
MongoDB: NoSQL database for storing user data and posts.
Passport: Authentication middleware for Node.js.
Multer: Middleware for handling file uploads.
UUID: Library for generating unique IDs.
Contributing
We welcome contributions from the community! Feel free to submit bug reports, feature requests, or even pull requests to improve the application
