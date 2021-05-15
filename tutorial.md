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