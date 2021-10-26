# ADVANCED ROUTING 


***SUMMARY***
- [Setting Up Our Environment](#setting-up-our-environment)
        - [Bootstrap](#bootstrap)
        - [Simple Form](#simple_form)
- [Advanced CRUD](#advanced-CRUD)
        - [Resources](#simple_form)   
        - [Scaffold](#scaffold)

# SETTING UP OUR ENVIRONMENT 

## BOOTSTRAP 

Today's challenges is all about using Advanced Routing and Advanced CRUD, it's best to have a better front-end, we can for now use **Bootstrap** until the next day course that will be about front-end in Rails. 

```console
yarn add bootstrap 
```

```console
rm app/assets/stylesheets/application.css
touch app/assets/stylesheets/application.scss

```

Add this on your ```application.html.erb``` layout to make sure bootstrap is more responsive

```html
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
```

## simple_form 

Let's re-use simple_form on these challenges as well 

Add this on your Gemfile :
```ruby 
gem 'simple_form'
```

Don't forget to bundle install & generate simple_form :wink:

```code 
bundle install
rails generate simple_form:install --bootstrap
```

# Advanced CRUD

## Resources

resources is another way to generate the 7 standard CRUD actions in your project

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :restaurants
end

```

```console
    > rails routes

      Prefix Verb   URI Pattern                     Controller#Action
    restaurants GET    /restaurants(.:format)          restaurants#index
                POST   /restaurants(.:format)          restaurants#create
 new_restaurant GET    /restaurants/new(.:format)      restaurants#new
edit_restaurant GET    /restaurants/:id/edit(.:format) restaurants#edit
     restaurant GET    /restaurants/:id(.:format)      restaurants#show
                PATCH  /restaurants/:id(.:format)      restaurants#update
                PUT    /restaurants/:id(.:format)      restaurants#update
                DELETE /restaurants/:id(.:format)      restaurants#destroy
```

## Scaffold

** :warning: Warning : DO NOT USE SCAFFOLD ON YOUR PROJECTS/CHALLENGES OF THE DAY  :warning:**

Scaffolding in programming is a temporary structure we generate for demonstrations and should not be used in real projects at all.

```code 


```


