---
layout: post
title:      "Deploying your React / Router / Redux and Rails API App to Heroku "
date:       2018-09-07 19:47:47 +0000
permalink:  deploying_your_react_router_redux_and_rails_api_app_to_heroku
---

Confusing, right? Well, in searching for a guide that would help me deploy my Flatiron final project to Heroku. I tried some shorter guides that other students had written in 2017, and no luck. I stumbled across this guide:
[A Rock Solid, Modern Web Stack—Rails 5 API + ActiveAdmin + Create React App on Heroku](https://blog.heroku.com/a-rock-solid-modern-web-stack)

What follows is a meta-guide to this amazing guide to get your Rails API with React frontend using React Router and Redux app working. All of the OPTIONAL steps relate to ActiveAdmin. You do not need to install ActiveAdmin of course, and it does make things more complicated. However, it is nice to be able to edit the database when the app is deployed with a nice dashboard. All of the bolded steps are not in the guide or are in a different order in the guide and relate to you copying your project code over instead of using their boilerplate code. 

This guide assumes you are installing a fresh copy of Rails and React. It makes it more complicated in the beginning, but I personally would rather deal with a fresh install rather than mysterious compile errors. Also, I found it refreshed my memory about all of the parts of the app, the rails part of which I had completed a while ago. 

This is important: do not use npm install during this process. This guide is written for Yarn. You will have compile errors if you npm instead.

You may want to copy and paste everything into another document that allows you to check all of these steps off since there are so many. 

* Create a new directory and use the rails generator to create an API only installation of Rails.
* Update the gitignore
* Check that rails is working
* **Import your controllers, classes, and routes config from your project.
* Change the production gem for the database to Postgres**
* OPTIONAL: Update application_controller.rb
* OPTIONAL: Create api_controller.rb
* OPTIONAL: Update all of your controllers to inherit from the api_controller
* OPTIONAL: Update config/application.rb
* OPTIONAL: Add the ActiveAdmin gem
* Bundle install your gems
* **Recreate your migrations, do not just copy and paste them over. **
* **Copy your seed file into the existing one that was created by ActiveAdmin or just create one yourself.**
* OPTIONAL: Edit the seed file to remove the if Rails.env.development? From the default user of activeadmin
* Run bin/rake db:migrate db:seed
* Start rails and make sure you can access your api, check ActiveAdmin if you installed it.
* Run create-react-app and install it in the client directory
* OPTIONAL: Remove the service workers from client/src/index.js
* Start the react app with Yarn
* Update your client/package.json
* **Copy over your React and Redux code**
*  **Skip the scaffolding** 
* OPTIONAL: Edit the activeadmin files to allow it to edit your database
* Make sure your API still works
* Create a procfile for development
* Add the start.rake code
* **Add your react and redux production dependencies using Yarn, not NPM. If you had been using NPM, you can see what your production dependencies were**
* Run the rake.start file and see if your application works. It most likely will fail the first time and you’ll have to fix whatever issue comes up. Routing might be failing, but that is coming later on. 
* Make a package.json in the root folder of the app, not /client
* Create a procfile for production
* OPTIONAL: create config/secrets.yml and add the keys
* Create the heroku app using the heroku CLI
* Add the heroku buildpacks
* Add the fallback_index_html method to application_controller
* Update the routes.rb file
* Install react-router-dom using yarn if you have not done so
* Create the NotFound component
* Add the NotFound component to your container router 
* Start the app and make sure non-routed URLs are getting redirected to your new 404 page
* **Git commit and push everything to Heroku
* Brace yourself for compile errors! It’s very unlikely that it will work the first time and you will probably need to debug the compile errors. If not, hooray! If there aren’t any compile errors, but your app isn’t displaying properly, check to make sure your api works on the web. If so, the issue is likely either a routing issue or a React Router issue. 
* Decide whether you want to commit this repo to github or not. 



