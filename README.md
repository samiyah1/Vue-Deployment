# Vue-Deployment
How To Deploy Vue using heroku
### 1.Create Your Heroku App
 Let’s create our Heroku app:
 ### heroku create <YOUR-PROJECT-NAME-HERE>
 * In order to avoid having Heroku install needless development dependencies when deploying later, set the NODE_ENV setting   * to production :
 ## heroku config:set NODE_ENV=production --app <YOUR-PROJECT-NAME-HERE>

## 2. Create a server.js and Build Your Site

* Since Vue is only a frontend library, the easiest way to host it and do things like serve up assets is to create a simple * Express friendly script that Heroku can use to start a mini-web server like  express:

 ### npm install express --save

* create a server.js file to your project’s root directory:

### In the server.js add this:

var express = require('express');
var path = require('path');
var serveStatic = require('serve-static');

app = express();
app.use(serveStatic(__dirname + "/dist"));

var port = process.env.PORT || 5000;
app.listen(port);

console.log('server started '+ port);

### IMPORTANT: What you probably noticed is that this will serve up a dist directory. dist is a predefined directory that Vue.js builds which is a compressed, minified version of your site. We’ll build this and then tell Heroku to run server.js so Heroku hosts up this dist directory:

### npm run build

You should see an output dist directory now.

Let’s test our server.js file by running it:

### node server.js

* Now go to http://localhost:5000 and make sure your app loads. This is the actual site Heroku will serve up.

* Lastly, we’ll have to edit our start script in package.json to start our node server, as Heroku will automatically look * * for this script when looking for how to run a node.js app.

// package.json
{
  "name": "<YOUR-PROJECT-NAME-HERE>",
  "version": "1.0.0",
  "description": "A Vue.js project",
  "author": "",
  "private": true,
  "scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js",
    "start": "node server.js",   <--- EDIT THIS LINE HERE 
...

## 3. Git Init and Add Your Heroku Remote Repository

Heroku allows us to push to a remote repository so we’ll first need to create our own git repository:

* git init

* Now let’s add our Heroku remote repository:

### heroku git:remote --app <YOUR-PROJECT-NAME-HERE>

Let’s keep our generated dist directory so that we can always keep a pristine copy of what we’ve deployed to Heroku by removing dist/ from .gitigore

.DS_Store
node_modules/
dist/  <--- REMOVE THIS LINE
npm-debug.log*
yarn-debug.log*
yarn-error.log*
test/unit/coverage
test/e2e/reports
selenium-debug.log

### Editor directories and files Make sure it contains this files bellow
.idea
*.suo
*.ntvs*
*.njsproj
*.sln

* Now, most importantly, let’s add and commit our code files:

### git add . && git commit -a -m "Adding files."

## 4. Push Your Code to Deploy!

 ### git push heroku master
