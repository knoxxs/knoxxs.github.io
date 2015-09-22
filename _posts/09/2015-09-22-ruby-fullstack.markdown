---
layout: post
title:  Ruby Fullstack
date:   2015-09-22 22:16:00
categories: programming ruby
tags: ruby rails fullstack course training learning skill
---

So today I decided to learn Ruby and the Ruby on Rails (RoR) to follow a easy, very fast (we call it agile to sound big) path to become full-stack developer. In the end my goal to be a developer in ruby so that after having a idea in my mind it should take me a weekend to develop it with all basic features which I can also deploy.

In this post I will be documenting the complete path I will be following to learn it.

## Learning Ruby

1. [To Ruby From Java](https://www.ruby-lang.org/en/documentation/ruby-from-other-languages/to-ruby-from-java/)
2. [Ruby in Twenty Minutes](https://www.ruby-lang.org/en/documentation/quickstart/)
3. [Try Ruby](http://tryruby.org/levels/1/challenges/0)


## Learning Rails

## Sample bookmark app - 12devs

I followed [this great post](http://12devs.co.uk/articles/writing-a-web-application-with-ruby-on-rails/) to make a sample bookmark app using RoR. It will give you a feel of how fast the development can be and how much automation is prebuilt in RoR flow. After this tutorial I got really excited as being a Java developer myself, I can only imagine to setup 1 library in java in the time I build the complete bookmark app in RoR. Though following this article I got into some issue once or twice but a simple google search helped me. Following are some the links which can also help you.

1. [`undefined method merge!` error](http://stackoverflow.com/questions/27611947/devise-raises-error-with-rails-4-2-upgrade)
2. [`users table not found` error](http://stackoverflow.com/questions/22582772/rake-dbmigrate-doesnt-work-rails-4-0-4)

Though this article is good to realize the power of ruby but its not the best to understand it. So I hav to find an article which explain things in deep.

## Ruby on rails tutorial - Michael Hartl
This is a big book but is quite famous on the web to teach you the complete package not the chit-chats. So lets get started:

## Chapter 1 - From zero to deploy

### Introduction

Rails is:

1. open source
2. a domain-specific language for writing web applications
3. adapts rapidly to new developments in web technology and framework design.

### Installation

```bash
gem install rails -v 4.2.2
```
### The first application

```bash
$ cd ~/workspace
$ rails _4.2.2_ new hello_app
```

The explicit include of the Rails version number (`_4.2.2_`) ensures that the same version of Rails is used. Running `rails new` automatically runs the bundle install command after the file creation is done.

#### Directory Structure

![A summary of the default Rails directory structure]({{ site.url }}assets/2015/09/learning-ruby/directory_structure.png)

#### Gemfile

_Versioning_: It helps to specify the version of dependencies:

1. Unless you specify a version number to the gem command, Bundler will automatically install the latest requested version of the gem: `gem 'sqlite3'`
2. `gem 'uglifier', '>= 1.3.0'`: This installs the latest version of the `uglifier` gem as long as it's greater than or equal to version `1.3.0`
3. `gem 'coffee-rails', '~> 4.0.0'`: This installs the `gem coffee-rails` as long as it’s newer than version `4.0.0` and not newer than `4.1`.

Converting the default Gemfile to use exact gem versions:

```ruby
source 'https://rubygems.org'

gem 'rails',                '4.2.2'
gem 'sass-rails',           '5.0.2'
gem 'uglifier',             '2.5.3'
gem 'coffee-rails',         '4.1.0'
gem 'jquery-rails',         '4.0.3'
gem 'turbolinks',           '2.3.0'
gem 'jbuilder',             '2.2.3'
gem 'sdoc',                 '0.4.0', group: :doc

group :development, :test do
  gem 'sqlite3',     '1.3.9'
  gem 'byebug',      '3.4.0'
  gem 'web-console', '2.0.0.beta3'
  gem 'spring',      '1.1.3'
end
```
 
Note that we’ve also taken this opportunity to arrange for the `sqlite3` gem to be included only in a development or test environment, which prevents potential conflicts with the database used by Heroku.

Install again: `$ bundle install`.

#### Deployment

>Thanks to running `rails new` and `bundle install`, we already have an application we can run.

Rails comes with a pre built server which can be used for testing and debugging.

```bash
$ rails server
```

#### Model-View-Controller (MVC)
Rails follows MVC pattern and you will see three different directories inside `app/` for each MVC.

#### Hello, world!
Change `app/controllers/application_controller.rb` to:

```ruby
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception

  def hello
    render text: "hello, world!"
  end
end
```

and `config/routes.rb` to:

```ruby
Rails.application.routes.draw do
  .
  .
  .
  # You can have the root of your site routed with "root"
  root 'application#hello'
  .
  .
  .
end
```

Rails router determines where to send requests that come in from the browser.

#### Version Control
` git mv README.rdoc README.md`

### Deploying
Heroku is defacto for ruby deployment. 

Heroku uses the PostgreSQL database, which means that we need to add the `pg gem` in the production environment to allow Rails to talk to Postgres. `sqlite3` is not supported by heroku. Note also the addition of the `rails_12factor` gem, which is used by Heroku to serve static assets such as images and stylesheets.

```ruby
group :production do
  gem 'pg',             '0.17.1'
  gem 'rails_12factor', '0.0.2'
end
```

To prepare the system for deployment to production, we run `bundle install` with a special flag to prevent the local installation of any production gems:

```
$ bundle install --without production
```

Because the only gems added are restricted to a production environment, right now this command doesn't actually install any additional local gems, but it’s needed to update `Gemfile.lock` with the pg and `rails_12factor` gems.

#### Heroku setup (one time)
1. Sign up
2. Install heroku : `brew install heroku`
3. `heroku login`
4. `heroku keys:add`

#### Heroku add deployment

```bash
$ heroku create
Creating damp-fortress-5769... done, stack is cedar
http://damp-fortress-5769.herokuapp.com/ | git@heroku.com:damp-fortress-5769.git
Git remote heroku added
```

The `heroku` command creates a new subdomain just for our application, available for immediate viewing. To deploy the application, the first step is to use Git to push the master branch up to Heroku:

```bash
$ git push heroku master
```

That's it the application is deployed.

```bash
$ heroku rename rails-tutorial-hello
```

## Chapter 2 - A toy app
This will explore the power of rails scaffolding. The toy app will consist of users and their associated microposts (thus constituting a minimalist Twitter-style app). 

### Planning the application

1. `rails _4.2.2_ new toy_app`
2. `cd toy_app/`
3. Replace `Gemfile`
    
    ```ruby
    source 'https://rubygems.org'
    
    gem 'rails',        '4.2.2'
    gem 'sass-rails',   '5.0.2'
    gem 'uglifier',     '2.5.3'
    gem 'coffee-rails', '4.1.0'
    gem 'jquery-rails', '4.0.3'
    gem 'turbolinks',   '2.3.0'
    gem 'jbuilder',     '2.2.3'
    gem 'sdoc',         '0.4.0', group: :doc
    
    group :development, :test do
      gem 'sqlite3',     '1.3.9'
      gem 'byebug',      '3.4.0'
      gem 'web-console', '2.0.0.beta3'
      gem 'spring',      '1.1.3'
    end
    
    group :production do
      gem 'pg',             '0.17.1'
      gem 'rails_12factor', '0.0.2'
    end
    ```
4. ` bundle install --without production`
5. git setup
6. `heroku create`
7. `git push heroku master`

### Models

![The data model for users.]({{ site.url }}assets/2015/09/learning-ruby/toy_app_user_model.png)

![The data model for microposts.]({{ site.url }}assets/2015/09/learning-ruby/toy_app_micropost_model.png)

### User

```bash
$ rails generate scaffold User name:string email:string
      invoke  active_record
      create    db/migrate/20140821011110_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
      invoke  resource_route
       route    resources :users
      invoke  scaffold_controller
      create    app/controllers/users_controller.rb
      invoke    erb
      create      app/views/users
      create      app/views/users/index.html.erb
      create      app/views/users/edit.html.erb
      create      app/views/users/show.html.erb
      create      app/views/users/new.html.erb
      create      app/views/users/_form.html.erb
      invoke    test_unit
      create      test/controllers/users_controller_test.rb
      invoke    helper
      create      app/helpers/users_helper.rb
      invoke      test_unit
      create        test/helpers/users_helper_test.rb
      invoke    jbuilder
      create      app/views/users/index.json.jbuilder
      create      app/views/users/show.json.jbuilder
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/users.js.coffee
      invoke    scss
      create      app/assets/stylesheets/users.css.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.css.scss
```

Note that there is no need to include a parameter for id; it is created automatically by Rails for use as the primary key in the database.

```bash
$ bundle exec rake db:migrate
==  CreateUsers: migrating ====================================================
-- create_table(:users)
   -> 0.0017s
==  CreateUsers: migrated (0.0018s) ===========================================
```

This simply updates the database with our new users data model. `Rake` is `make` for ruby. To see all the database tasks use `$ bundle exec rake -T db`. To see all the tasks rake can do use `$ bundle exec rake -T`.

>To ensure that the command uses the version of `Rake` corresponding to our `Gemfile`, we need to run rake using `bundle exec`.
 
#### User tour

![The correspondence between pages and URLs for the Users resource.]({{ site.url }}assets/2015/09/learning-ruby/toy_app_user_tour.png)

#### User MVC in action

![A detailed diagram of MVC in Rails.]({{ site.url }}assets/2015/09/learning-ruby/toy_app_user_mvc.png)

#### User routes

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :users
  .
  .
  .
end
```

This maps all the urls specified in the table above. The `:user` is a symbol in ruby. To make the root page of the app (`/`) to print all the users do the following:

```ruby
#config/routes.rb
Rails.application.routes.draw do
  resources :users
  root 'users#index'
  .
  .
  .
end
```

#### User controller

```ruby
# app/controllers/users_controller.rb
class UsersController < ApplicationController
  .
  .
  def index
    .
    .
  end

  def show
    .
    .
  end

  def new
    .
    .
  end

  def edit
    .
    .
  end

  def create
    .
    .
  end

  def update
    .
    .
  end

  def destroy
    .
    .
  end
end
```

![RESTful routes provided by the Users resource]({{ site.url }}assets/2015/09/learning-ruby/toy_app_user_controller.png)

#### User Model

```ruby
# app/models/user.rb
class User < ActiveRecord::Base
end
```

Then this model is used in controller to talk to db:

```ruby
#app/controllers/users_controller.rb
class UsersController < ApplicationController
  .
  .
  .
  def index
    @users = User.all
  end
  .
  .
  .
end
```

Once the `@users` variable is defined, the controller calls the view

#### User View
Variables that start with the `@` sign, called instance variables, are automatically available in the views; in this case, the `index.html.erb`:

```html
<h1>Listing users</h1>

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th colspan="3"></th>
    </tr>
  </thead>

<% @users.each do |user| %>
  <tr>
    <td><%= user.name %></td>
    <td><%= user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, method: :delete,
                                     data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
</table>

<br>

<%= link_to 'New User', new_user_path %>
```

### Microposts

Do the same thing with microposts:

1. `$ rails generate scaffold Micropost content:text user_id:integer`
2. `$ bundle exec rake db:migrate`
3. Routes: `resources :microposts`
4. Check the controller, model and view

#### Adding validation

```ruby
# app/models/micropost.rb
class Micropost < ActiveRecord::Base
  validates :content, length: { maximum: 140 }, presence: true
end
```

### User-Micropost relationship
One of the most powerful features of Rails is the ability to form associations between different data models.

```ruby
# app/models/user.rb
class User < ActiveRecord::Base
  has_many :microposts
end
```

```ruby
# app/models/micropost.rb
class Micropost < ActiveRecord::Base
  belongs_to :user
  validates :content, length: { maximum: 140 }
end
```

### Rails console
It is a useful tool for interacting with Rails applications.

```bash
$ rails console
>> first_user = User.first
=> #<User id: 1, name: "Michael Hartl", email: "michael@example.org",
created_at: "2014-07-21 02:01:31", updated_at: "2014-07-21 02:01:31">
>>
>> first_user.microposts
=> [#<Micropost id: 1, content: "First micropost!", user_id: 1, created_at:
"2014-07-21 02:37:37", updated_at: "2014-07-21 02:37:37">, #<Micropost id: 2,
content: "Second micropost", user_id: 1, created_at: "2014-07-21 02:38:54",
updated_at: "2014-07-21 02:38:54">]
>>
>> micropost = first_user.microposts.first    # Micropost.first would also work.
=> #<Micropost id: 1, content: "First micropost!", user_id: 1, created_at:
"2014-07-21 02:37:37", updated_at: "2014-07-21 02:37:37">
>>
>> micropost.user
=> #<User id: 1, name: "Michael Hartl", email: "michael@example.org",
created_at: "2014-07-21 02:01:31", updated_at: "2014-07-21 02:01:31">
>>
>> exit
```

### Inheritance

```ruby
User, Micropost < ActiveRecord::Base

UsersController, MicropostsController < ApplicationController < ActionController::Base
```

`ActionController::Base` includes the ability to manipulate model objects, filter inbound HTTP requests, and render views as HTML.

### Deploying

1. `git push heroku`
2. `heroku run rake db:migrate`
## References

1. [To Ruby From Java](https://www.ruby-lang.org/en/documentation/ruby-from-other-languages/to-ruby-from-java/)
2. [Ruby in Twenty Minutes](https://www.ruby-lang.org/en/documentation/quickstart/)
3. [Try Ruby](http://tryruby.org/levels/1/challenges/0)
4. [Writing a web application with Ruby on Rails - Bookmark app](http://12devs.co.uk/articles/writing-a-web-application-with-ruby-on-rails/)
5. [apeacox/12dos-bookmarks](https://github.com/apeacox/12dos-bookmarks)
6. [rake db:migrate doesn't work (Rails 4.0.4)](http://stackoverflow.com/questions/22582772/rake-dbmigrate-doesnt-work-rails-4-0-4)
7. [Devise raises error with Rails 4.2 upgrade](http://stackoverflow.com/questions/27611947/devise-raises-error-with-rails-4-2-upgrade)
8. [Ruby on rails tutorial - Michael Hartl](https://www.railstutorial.org/book/)