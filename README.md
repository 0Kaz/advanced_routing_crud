# ADVANCED ROUTING 


***SUMMARY***
- [Setting Up Our Environment](#setting-up-our-environment)
        - [Bootstrap](#bootstrap)
        - [Simple Form](#simple_form)
- [Advanced CRUD](#advanced-CRUD)
        - [Resources](#simple_form)   
        - [Scaffold](#scaffold)
        - [Seeding](#seeding)
        - [Destroy ALL](#destroy_all)
        - [Route for 5 stars restaurants Example](#route-for-5-stars-restaurants-example)
        - [Adding a chef example](#adding-a-chef-example)
- [Nested Resources](#nested-resources)

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

resources is another way to generate the 7 standard RESTful routes in your project

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

rails g scaffold Restaurant name address stars:integer

```


## Seeding

Let's seed in Rails for today, we can use Faker to fill up our DB.

```ruby 
gem 'faker'
```

```console
bundle install
```

```ruby
#db/seeds.rb
require 'faker'

puts "Clean database..."
Restaurant.destroy_all

puts "Creating restaurants"

12.times do 
    Restaurant.create!(name: Faker::Restaurant.name , address: Faker::Address.street_address , stars: rand(1..5))
end

puts "Database added"
```

## destroy_all

What if we wanted to destroy all Restaurant data we just inserted ?  

```console
    rails c
    irb(main):001:0> Restaurant.destroy_all
       (1.0ms)  SELECT sqlite_version(*)
  Restaurant Load (0.6ms)  SELECT "restaurants".* FROM "restaurants"
  TRANSACTION (0.1ms)  begin transaction
  Restaurant Destroy (0.8ms)  DELETE FROM "restaurants" WHERE "restaurants"."id" = ?  [["id", 1]]
  TRANSACTION (2.1ms)  commit transaction
  TRANSACTION (0.1ms)  begin transaction
  Restaurant Destroy (1.0ms)  DELETE FROM "restaurants" WHERE "restaurants"."id" = ?  [["id", 2]]

  .....ETC

```

## Route for 5 stars restaurants Example

What if we go to this route ``` GET /restaurants/top ``` and get a list of 5 stars restaurants ? 


**Routes**

```ruby
Rails.application.routes.draw do
  resources :restaurants do
    collection do
      get :top
    end
  end
end
```

```console
        > rails routes
         Prefix Verb   URI Pattern                     Controller#Action
**top_restaurants GET    /restaurants/top(.:format)      restaurants#top**
    restaurants GET    /restaurants(.:format)          restaurants#index
                POST   /restaurants(.:format)          restaurants#create
 new_restaurant GET    /restaurants/new(.:format)      restaurants#new
edit_restaurant GET    /restaurants/:id/edit(.:format) restaurants#edit
     restaurant GET    /restaurants/:id(.:format)      restaurants#show
                PATCH  /restaurants/:id(.:format)      restaurants#update
                PUT    /restaurants/:id(.:format)      restaurants#update
                DELETE /restaurants/:id(.:format)      restaurants#destroy
```

You can also filter your routes by specifying the action you are looking for: 

```console 
        > rails routes -g top
         Prefix Verb URI Pattern                Controller#Action
top_restaurants GET  /restaurants/top(.:format) restaurants#top
```

**Controller**
```ruby
  ...
  def top
    @restaurants = Restaurant.where(stars: 5)
  end
  ...
```
**View**

```ruby
#app/views/restaurants/top.html.erb
<h1>Top 5 stars Restaurants</h1>

<% @restaurants.each do |res| %>
    <h1><%= res.name %></h1>
<% end %>
```

## Adding a chef example

```code
rails g migration AddChefNameToRestaurants chef_name:string
rails db:migrate
```

**Routes**
```ruby
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  resources :restaurants do
    collection do
      get :top
    end
    member do 
      get :chef
    end
  end
end

```


```code

        > rails routes -g chef
         Prefix Verb URI Pattern                     Controller#Action
chef_restaurant GET  /restaurants/:id/chef(.:format) restaurants#chef
```

**Controller**

```ruby
    before_action :set_restaurant, only: %i[ show edit update destroy **chef** ]

  ...
  def chef
    @chef_name = @restaurant.chef_name
  end
  ...

```
# Nested resources

Let's add a review to our restaurant example 