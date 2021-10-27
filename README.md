# ADVANCED ROUTING Cheatsheets


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
      - [Adding a review to restaurant example](#adding-a-review-to-restaurant-example)
      - [Destroying a review to a restaurant example](#destroying-a-review-to-a-restaurant-example)
 - [Challenge Yelp MVP Tips](#challenge-yelp-mvp-tips)
        

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

Again, please do not use scaffolding in the upcoming challenges and your projects ! :pray:	

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
top_restaurants GET    /restaurants/top(.:format)      restaurants#top
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
    before_action :set_restaurant, only: %i[ show edit update destroy chef ]

  ...
  def chef
    @chef_name = @restaurant.chef_name
  end
  ...

```
# Nested resources

## Adding a review to restaurant example
Let's add a review to our restaurant example : 

**Generate our model + controller**

```code
rails g model Review content:text restaurant:references
rails g controller reviews
rails db:migrate
```

**Routes**

```ruby
Rails.application.routes.draw do
  resources :restaurants do
    resources :reviews, only: [ :new, :create ]
  end
end
end
```

Let's get the ```rails routes``` of our new controller only
```code
              > rails routes -c reviews
               Prefix Verb URI Pattern                                       Controller#Action
   restaurant_reviews POST /restaurants/:restaurant_id/reviews(.:format)     reviews#create
new_restaurant_review GET  /restaurants/:restaurant_id/reviews/new(.:format) reviews#new
```

**Controller**

```ruby
class ReviewsController < ApplicationController

    def new 
        @restaurant = Restaurant.find(params[:restaurant_id])
        @review = Review.new 
    end
end
```


**Restaurant Model**

```ruby
#app/models/restaurant.rb
class Restaurant < ApplicationRecord
  has_many :reviews, dependent: :destroy
end
```

**Review Model**

```ruby
class Review < ApplicationRecord
  belongs_to :restaurant
end
```

**View**

```ruby
<!-- app/views/restaurants/show.html.erb -->

<%= link_to 'Edit', edit_restaurant_path(@restaurant) %> |
<%= link_to 'Back', restaurants_path %>
<%= link_to 'Review this restaurant', new_restaurant_review_path(@restaurant) %>
```

```ruby
<!-- app/views/reviews/new.html.erb -->

<%= simple_form_for [@restaurant, @review] do |f| %>
    <%= f.input :content %>
    <%= f.submit "Submit review", class: 'btn btn-primary' %>
<% end %>

<!--form_for([@restaurant, @review])	POST restaurant_reviews_path-->

```

**Review controler 2**

```ruby
   def create 
        @review = Review.new(review_params)
        @restaurant = Restaurant.find(params[:restaurant_id])
        @review.restaurant = @restaurant 
        @review.save 
        redirect_to restaurant_path(@restaurant)
    end

    private 

    def review_params
        params.require(:review).permit(:content)
    end

```


***Review view***

```ruby
<!-- app/views/restaurants/show.html.erb -->

<ul class="list-group">
  <% @restaurant.reviews.each do |review| %>
    <li class="list-group-item"><%= review.content %></li>
  <% end %>
</ul>
```
**Validation in review Model**

```ruby
 class Review < ApplicationRecord
  belongs_to :restaurant

  validates :content, presence: true
end
```

## Destroying a review to a restaurant example


Before we start configuring our routes, remember that we don't nest routes when models are **related**

:warning: We have to avoid deep levels of nesting.

For a deep dive in shallow nesting, check out this link :point_right: https://guides.rubyonrails.org/routing.html#shallow-nesting

**Routes**

```ruby
Rails.application.routes.draw do
  resources :restaurants do
    resources :reviews, only: [ :new, :create ]
  end
  resources :reviews, only: [ :destroy ]
end
```

**Controller**

```ruby
...
  def destroy
    @review = Review.find(params[:id])
    @review.destroy
    redirect_to restaurant_path(@review.restaurant)
  end
  ...
```

**View**
```ruby
<ul class="list-group">
  <% @restaurant.reviews.each do |review| %>
     <li class="list-group-item">
      <%= review.content %>
      <%= link_to "Remove",
                  review_path(review),
                  method: :delete,
                  data: { confirm: "Are you sure?" } %>
    </li>
  <% end %>
</ul>
```

## Challenge Yelp MVP tips

```Guess what? Rake is BACK``` :green_heart: :t-rex:	

In this challenge, you will have the perfect opportunity to use nested resources + CRUD. 

I will recommend you guys to read the whole exercice so that you can have a little idea of the big picture before starting coding.

One more advice I would share, don't start coding right away until you finish reading your whole challenge, our challenges in the Rails track and the upcoming ones are constructive through guidelines.

We will build a two-model Rails app with a restaurant and anonymous reviews.

1. We need then 2 models (review, restaurants)
2. Generate their controllers
3. Configure their routes
4. Set up ```validations``` in their model
5. Seeding !
6. Configure their route
7. Set up their controllers
8. And finally work on their views

### Don't leave your buddy behind and work together :pray: , like Helen Keller once said: "Alone we can do so little; together we can do so much." 



