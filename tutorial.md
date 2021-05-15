# Deploy a back-end and front-end app using Heroku and Netlify
Posted May 15, 2021

I'm was working on a project which needs to be deployed online. 
- After research and testing, this tutorial emerged.

I want to deploy a minimal modern webapp (backend and frontend) on two different platforms, to get the most out of them.

Today, developers like to make a webapp as SPA (Single Page Application) to consume data by using an API (Application Program Interface). 
- This splits the complexity and responsibility in different codebases.
- **Heroku** is a good option to deploy an api-based backend app in seconds, and **Netlify** is a much better solution for frontend app (build system, CDN and more).

**Steps and source code to start from zero to a minimal remote and working app.**

- This tutorial assumes you already have Node.js installed and experience on running commands from within the CLI (Terminal or bash interface).

## The backend

I use ```Node.js``` and ```Express.Js``` to develop this api-only backend and here the steps to set it up:

- Create a new Github repository, check it out on your local computer, open that folder with your CLI.
- Run ```npm init``` to set up a ```package.json``` file.
- Run ```npm i express``` to install ExpressJs.
- Create a new ```index.js``` file with the following code:

```
const express = require('express')
const app = express()

app.get('/api', (req, res) => {
  res.status(200).json({api: 'version 1'})
})

app.listen(3000, () => console.log('server started'))```

- In ```package.json``` add the command ```"start": "node index.js"``` in ```"script"``` portion.
- Run your backend with npm ```start``` and test it with your browser ```http://localhost:3000/api```, you should see the response.

## The frontend

Now it's time for the front end part. 
- This example is with  **Vue**. The goal of the app is to make a remote request to a server (the backend) in order to get some data.

Here are the steps:

 - Create a new Github repository, check it out on your local computer, open that folder with your CLI.
 - Run ```npm init``` to set up a ```package.json``` file.
 - Run ```npm i vue``` to install VueJs.
 - Create a new ```index.html``` file with the following code:

 ```
 <!DOCTYPE html>
  <html>
  <head>
    <title></title>
  
    <script type="text/javascript" src="node_modules/vue/dist/vue.js"></script>
    <script type="text/javascript" src="node_modules/axios/dist/axios.js"></script>
  </head>
  <body>
    <div id="app">
      <textarea v-model="src"></textarea>
    </div>
  
    <script type="text/javascript">
      new Vue({
        el: '#app',
        data: {
            src: ''
        },
        mounted(){
          var vm = this
          axios.get('http://localhost:3000/api')
            .then(data => {
              vm.src = JSON.stringify(data.data)
            })
        }
      })
    </script>
  </body>
  </html>
  ```
  - Test with a local webserver (http-server or similar).
  
You should get an error in the console ```Access-Control-Allow-Origin``` because the security policy of the browser. 
- To fix it we need to add something on the backend installing the ```cors``` module:
```
const cors = require('cors')
app.use(cors())
```
**Boom!!** The backend can now handle the cross origin request properly. When you run the frontend with ```http-server``` you can reach it with ```http://localhost:8080``` while the backend uses ```http://localhost:3000``` as domain name.

While this may work on your local machine , it's time todeploy them so the world can you your creations.

## Deploy the backend on Heroku

The steps to deploy our backend on Heroku are minimal.

- Change the port setting in the ```index.js``` code:
```
const port = process.env.PORT || 3000
...
app.listen(port, () => console.log('server started on port', port))
```
This is very important otherwise the server app won't start.

On your Heroku Dashboard; 
- create a new App,
- connect it to your Github repository, 
- click the manual deploy and check it with the public URL provided by Heroku.
Your backend should be up and running at Heroku scale!

## Deploy the frontend on Netlify

To deploy a simple website on Netlify usually requires just a repository connection. 
- In our example app, we need to set up also a build command because we're using Npm modules, which won't be published by Netlify:

  - On your Netlify Dashboard, create a new App and connect it to your Github repository.
  - In the build setting, set the build command with ```npm start``` and the deploy folder with ```dist```.
  - In the ```package.json``` file, add in the ```"Script"```, this command:

```mkdir dist && 
cp node_modules/vue/dist/vue.js dist/vue.js && 
cp node_modules/axios/dist/axios.js dist/axios.js && 
cp index.html dist/index.html
```
We will also edit the script import relative path in the HTML file:
```
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript" src="axios.js"></script>
```
In the javascript section, the localhost URL needs to be changed with the Heroku one:

```
// this url must be the Heroku remote one
axios.get('http://localhost:3000/api')
// change to something like
axios.get('https://poc-express-api-heroku.herokuapp.com/api')
```

If everything has been done properly, your webapp made by an API-based backend running on **Heroku**, with a Vue-based SPA running on **Netlify**, should work perfectly.

Here the code repositories:

- Backend
- Frontend

Fantastic!!!!