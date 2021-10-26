# ADVANCED ROUTING 


***SUMMARY***
- []()
        - []()


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