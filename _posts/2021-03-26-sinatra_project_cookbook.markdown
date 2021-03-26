---
layout: post
title:      "Sinatra Project — CookBook"
date:       2021-03-26 15:22:22 -0400
permalink:  sinatra_project_cookbook
---

I decided that for my Sinatra MVC/CRUD project I would create an app to store and share recipes. I consider myself to be an avid cook so I’m always either creating new recipes or looking up something interesting to make. Noting down my recipes in a sticky notes or keeping a note book isn’t always ideal so my other option was to note it down in my notes app on my iPhone. This prevented me from loosing or spilling on my perishable paper notes but it didn’t help with organization. Another aspect I wanted to implement was being able to see what other people are coming up with. Of course I could go on line and simply search up different recipes but my problem with that was you are always left with having to read an essay in order to find the recipe hidden within. I wanted to create an app to store all your recipes digitally and to also be able to see other peoples recipes without out any story attached to them and that is what the CookBook app is. This app is the first of its kind that I’ve created and therefore is rather basic but holds the potential to become so much more. Perhaps in the future I would like to add a “Show Me What I Can Make” feature in which a user can list items they have lying around the house and the app will generate what can be made with those ingredients by scraping the database of recipes created on the app.

### Project Setup 

Instead of creating the entire file structure from scratch I decided to use [Corneal](https://github.com/thebrianemory/corneal).
![1](https://imgur.com/a/jFcef3B)
The next step was to create a new repository on GitHub and follow the instructions to push an existing repository from the command line. Before copying and pasting the instructions from GitHub first run these commands in order git init, git add . , and git commit -m “message”.
![2](https://imgur.com/a/y0PovnM)
![3](https://imgur.com/a/jn7WlZV)
The first aspect I focused on was the models and database, followed by views and then controllers. In many instances I was working between the controllers, views, and models simultaneously but in terms of creation of the files I did so in the order previously mentioned. 

### Models & Database

I created two models, first was the user model and the second was the recipe model. I simply created the class user and class recipe in separate files and inherited each from ActiveRecord::Base. Once the shell of each model was set up I moved over to the database.
The first thing I did was create migrations which builds the database and the structure of the database based on the models. I did so by running rake db:create_migration NAME=create_users_table in my terminal. This created the migration and then I just needed to fill it in with the columns in the table. Those columns are name, email, and password_digest.
![4](https://imgur.com/a/NNHrIDK) ![5](https://imgur.com/a/KEUSxFr)
I used password_digest instead of just password ignorer to encrypt the password through bcrypt which lives in the gemfile.
Side Note : running rake -T will show you all the commands available through rake and they all come from ‘sinatra/activerecord/rake’ located in our rakefile.
I repeated the same process for the recipes table but with different attributes; name, ingredients, instructions, and user_id.
Next I needed to fill in models. The first thing I did was to add has_secured_password to my users model which allows us to use an ActiveRecord method called authenticate. Next was to create the relationships between each model, so the user has many recipes while a recipe belongs to a user.
![6](https://imgur.com/a/J6UQpUl) ![7](https://imgur.com/a/Hvlxl84)
Once all this is completed it is important to run rake db:migrate to actually create the database and to create the schema. 

### Controllers & Views

The application controller is the first controller I worked on. In this controller I first enabled sessions and set session_secret in the configure block. I then created a users controller to start creating signup and login routes which render to the corresponding erb files in the views folder.
![8](https://imgur.com/a/34ab9My) ![9](https://imgur.com/a/7aJgbHN)
As you can see I stated that if the fields of name, email and password are not blank to give the user an id and to redirect the user to users show page and if the name, email and password fields are left blank to simply redirect to the same page—signup. The same MVC methodology follows the rest of my code such as login, logout for users and create, read, update, and delete (CRUD) for my recipes. 
I ran into an interesting issue when creating the edit and delete routes of the recipe controllers. For some reason when I would create the route, for example get ‘/recipes/:id/edit’ and same for delete ‘/recipes/:id’ I would keep getting the infamous “Sinatra doesn’t know this ditty”. I tried everything until I realized what Sinatra was telling me to do. In both cases it wanted my to add another “recipe” route to the entire chain so instead of get ‘/recipes/:id/edit’ it wanted get ‘recipes/recipes/:id/edit’. Once I made this change everything starting working but I’m still somewhat confused by this and my only guess is that I am already in the recipe when trying to edit or delete it needs to render a new recipe page in order to do so?

### Nav Bar

Having a navigation bar seemed like a good addition to my project and wasn’t a major task to tackle. My navigation bar lives within my layout.erb file under the views folder.
![10](https://imgur.com/a/rhKuwXo)
I placed the nav class under my footer class as I thought this would be the most appealing. The nav bar has four different functions. It has links to My Home which is the users show page,  a link to recipes which holds everyones recipes in a feed form, a link to create recipes which renders to the recipes/new.erb form and lastly a link to logout which clears the session and redirects the user back to the welcome page or login/signup page. 
![11](https://imgur.com/a/C98yz1b)

### Flash Messages

Adding flash messages to my project was a stretch goal but I’m very glad I decided to add them in. I created two types of flash messages one for positive messages and one for errors. Messages (flash[:message]) have green text and errors (flash[:error]) have red text. These are then implemented throughout my code giving the user messages letting them know what’s going on. For example once they’ve successfully created a recipe a temporary message will pop up in green saying "Tasty!! Your recipe was successfully created!” And when an error has occurred such as not filling in all the text areas when signing up, a red message will pop up saying "All fields are required to created account”.
![12](https://imgur.com/a/2oGtFjf)

### Helper Methods

In order to keep my code neat and clean I decided to add in helper methods pretty much anywhere I was repeating the same code. All in all I created six helper methods, four of which live in my application_controller and two which live in the recipe model. The four in the application_controller are used to replace code that is used more than once in the users_controller such as logged_in? which makes sure the user is logged in, authorized_to_edit which makes sure the user is trying to edit their own recipe, current_user which makes sure you can only edit and delete your own recipe, and redirect_if_not_logged_in which will redirect you to the welcome page if you are not logged in. The other two helper methods are formatted_created_at and formatted_updated_at which simply display the the time and date in which a recipe was created at and updated at in a much easier way rather than using UTC time (refer to picture 7). These are displayed in the recipe index page and show page of the recipe.
![13](https://imgur.com/a/0yq780p)

