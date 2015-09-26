---
layout: post
title:  Ruby on rails tutorial - Michael Hartl - Chap 5-8
date:   2015-09-24 16:35:00
categories: programming rails learning
tags: ruby rails learning full-stack tutorial notes
---

These are my notes for the book: [Ruby on rails tutorial - Michael Hartl](https://www.railstutorial.org/book/) for chapter 5-8. Previous chapters are covered in [Ruby on rails tutorial - Michael Hartl - Chap 1-4](http://knoxxs.github.io/programming/rails/learning/2015/09/22/ruby-on-rails-tutorial-michael-hartl-chap_1_4/) 

## Chapter 5 - Filling in the layout

### Site navigation

```html
<!-- app/views/layouts/application.html.erb -->
<!DOCTYPE html>
<html>
  <head>
    <title><%= full_title(yield(:title)) %></title>
    <%= stylesheet_link_tag 'application', media: 'all',
                                           'data-turbolinks-track' => true %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
    <%= csrf_meta_tags %>
    <!--[if lt IE 9]>
      <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
      </script>
    <![endif]-->
  </head>
  <body>
    <header class="navbar navbar-fixed-top navbar-inverse">
      <div class="container">
        <%= link_to "sample app", '#', id: "logo" %>
        <nav>
          <ul class="nav navbar-nav navbar-right">
            <li><%= link_to "Home",   '#' %></li>
            <li><%= link_to "Help",   '#' %></li>
            <li><%= link_to "Log in", '#' %></li>
          </ul>
        </nav>
      </div>
    </header>
    <div class="container">
      <%= yield %>
    </div>
  </body>
</html>
```

`link_to`: to create links, the first argument to link_to is the link text, while the second is the URL. The third argument is an options hash, in this case adding the CSS id logo to the sample app link.

The second link_to shows off the image_tag helper, which takes as arguments the path to an image and an optional options hash, in this case setting the alt attribute of the image tag using symbols. ails will automatically find any images in the `app/assets/images/` directory using the asset pipeline . To make the effects of image_tag clearer, let’s look at the HTML it produces: `<img alt="Rails logo" src="/assets/rails-9308b8f92fea4c19a3a0d8385b494526.png" />`. Here the string `9308b8f92fea4c19a3a0d8385b494526` (which will differ on your system) is added by Rails to ensure that the filename is unique, which causes browsers to load images properly when they have been updated (instead of retrieving them from the browser cache).

```html
<!-- app/views/static_pages/home.html.erb -->
<div class="center jumbotron">
  <h1>Welcome to the Sample App</h1>

  <h2>
    This is the home page for the
    <a href="http://www.railstutorial.org/">Ruby on Rails Tutorial</a>
    sample application.
  </h2>

  <%= link_to "Sign up now!", '#', class: "btn btn-lg btn-primary" %>
</div>

<%= link_to image_tag("rails.png", alt: "Rails logo"),
            'http://rubyonrails.org/' %>
```

## Bootstrap and custom CSS

The Bootstrap framework natively uses the Less CSS language for making dynamic stylesheets, but the Rails asset pipeline supports the (very similar) Sass language by default (Section 5.2), so bootstrap-sass converts Less to Sass and makes all the necessary Bootstrap files available to the current application.

```ruby
# Gemfile
source 'https://rubygems.org'

gem 'rails',                '4.2.2'
gem 'bootstrap-sass',       '3.2.0.0'
.
.
```

Although rails generate automatically creates a separate CSS file for each controller, it’s surprisingly hard to include them all properly and in the right order, so for simplicity we’ll put all of the CSS needed for this tutorial in a single file.

```bash
touch app/assets/stylesheets/custom.css.scss
```

Inside the file for the custom CSS, we can use the @import function to include Bootstrap (together with the associated Sprockets utility)

```css
# app/assets/stylesheets/custom.css.scss
@import "bootstrap-sprockets";
@import "bootstrap";
```

The directory `app/assets/stylesheets/` is part of the asset pipeline, and any stylesheets in this directory will automatically be included as part of the `application.css` file included in the site layout.
## Reference

1. [Ruby on rails tutorial - Michael Hartl - Chapter 5](https://www.railstutorial.org/book/filling_in_the_layout)