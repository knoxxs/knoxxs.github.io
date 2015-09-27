---
layout: post
title:  FreeCodeCamp
date:   2015-09-13 15:43:00
categories: programming web
tags: programming web freeCodeCamp tutorial training full-stack js bootstrap html css node font-awesome animated
---

I started doing FreeCodeCamp 4-5 days back. Its a really great resource. I think every programming community should provide such training opportunity. There are many new things I am learning while doing it. So I thought it would be better to document some of them so that I can refer to this whenever I forgets something.

## Font-Awesome

[Font awesome]((https://fortawesome.github.io/Font-Awesome/)) is a library for *icon-fonts*. This made me think how icon-fonts actually works. So I had a brief discussion on same with [Vipul Garg](https://twitter.com/vipul_261). The gist is documented below.

## png vs svg vs icon-fonts

*png*: Need different image for different resolutions. Sprites help to optimize.
*svg*: A bit complex. Sprites help to optimize. Not supported by old browsers.
*icon-fonts*:

1. single color
2. behave as fonts (so sometimes you have to some extra styling, in case of image there are some predefined styling supported by browser)
3. browser handles everything: downloading the font sprite, rendering it and stuff.

## How icon-fonts works

They work using pseudo-elements. As there are static and don't need any interaction we use pseudo elements to add icon fonts to DOM. This also saves us from any DOM elements. To support icon-fonts we only need CSS, no js is needed.

Fonts works by defining a file in which there is a mapping between `code` and an `svg` image. icon-font libs provide to a css class for each icon. In that class the icon's `code` is set in the `content` css property for the `:before` pseudo html element on which the class is applied.

Bonus: Awesome things people have made with pseudo elements : [A Single Div](http://a.singlediv.com/)

## Animated
[Animated](https://daneden.github.io/animate.css/) is a css lib which help you use the power css animations.

## Bootstrap
[Bootstrap](http://getbootstrap.com/) is the most popular HTML, CSS, and JS framework for developing responsive, mobile first projects on the web.

Bootstrap is too big. They don't have any official reference guide. The closes tutorial to the reference guide I was able to find was [TutorialsPoint - Bootstrap](http://www.tutorialspoint.com/bootstrap/index.htm)

Here are the some important points about it:

Bootstrap includes following:

1. Responsive design
2. Scaffolding
3. CSS
4. Components
5. JavaScript Plugins
6. Customize

### Grid
Bootstrap includes a responsive, mobile first fluid grid system that appropriately scales up to 12 columns as the device or viewport size increases. Grid systems are used for creating page layouts through a series of rows and columns that house your content. Rows must be placed within a `.container` class for proper alignment and padding. Predefined grid classes like .row and .col-xs-4 are available for quickly making grid layouts. Grid columns are created by specifying the number of twelve available columns you wish to span. For example, three equal columns would use three `.col-xs-4`.

![Bootstrap Grid Options]({{ site.url }}assets/freecodecamp/bootstrap-grid.png)

Use class `.container` to wrap a page's content and easily center the content.

```html
<!-- Sample gid structure -->
<div class="container">
   <div class="row">
      <div class="col-*-*"></div>
      <div class="col-*-*"></div>      
   </div>
</div>
```

And there are many more options like column resets, offset columns, nesting columns, column ordering which can be found at [Bootstrap Grid System](http://www.tutorialspoint.com/bootstrap/bootstrap_grid_system.htm)

### Common html 

```html
<!-- Headings -->
<h1>I'm Heading1 h1. <small>I'm secondary Heading1 h1</small></h1> 
<h2>I'm Heading2 h2. <small>I'm secondary Heading2 h2</small></h2>
<h3>I'm Heading3 h3. <small>I'm secondary Heading3 h3</small></h3>
<h4>I'm Heading4 h4. <small>I'm secondary Heading4 h4</small></h4>
<h5>I'm Heading5 h5. <small>I'm secondary Heading5 h5</small></h5>
<h6>I'm Heading6 h6. <small>I'm secondary Heading1 h6</small></h6>

<h2>Lead Example</h2>
<p class="lead">This is an example paragraph demonstrating the use of lead body copy. This is an example paragraph demonstrating the use of lead body copy.This is an example paragraph demonstrating the use of lead body copy.This is an example paragraph demonstrating the use of lead body copy.This is an example paragraph demonstrating the use of lead body copy.</p>

<!-- Text-->
<small>This content is within <small> tag</small><br>
<strong>This content is within <strong> tag</strong><br>
<em>This content is within <em> tag and is rendered as italics</em><br>
<p class="text-left">Left aligned text.</p>
<p class="text-center">Center aligned text.</p>
<p class="text-right">Right aligned text.</p>
<p class="text-muted">This content is muted</p>
<p class="text-primary">This content carries a primary class</p>
<p class="text-success">This content carries a success class</p>
<p class="text-info">This content carries a info class</p>
<p class="text-warning">This content carries a warning class</p>
<p class="text-danger">This content carries a danger class</p>

<!-- Abbreviations-->
<abbr title="World Wide Web">WWW</abbr><br>
<abbr title="Real Simple Syndication" class="initialism">RSS</abbr>

<!-- Address-->
<address>
  <strong>Some Company, Inc.</strong><br>
  007 street<br>
  Some City, State XXXXX<br>
  <abbr title="Phone">P:</abbr> (123) 456-7890
</address>

<address>
  <strong>Full Name</strong><br>
  <a href="mailto:#">mailto@somedomain.com</a>
</address>

<!-- Blockquote -->
<blockquote><p>
This is a default blockquote example. This is a default blockquote example. This is a default blockquote example.This is a default blockquote example. This is a default blockquote example.</p></blockquote>
<blockquote>This is a blockquote with a source title.<small>Someone famous in <cite title="Source Title">Source Title</cite></small></blockquote>
<blockquote class="pull-right">This is a blockquote aligned to the right.<small>Someone famous in <cite title="Source Title">Source Title</cite></small></blockquote>

<!-- Lists -->
<h4>Example of Ordered List</h4>
<ol>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
</ol>
<h4>Example of UnOrdered List</h4>
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
</ul>
<h4>Example of Unstyled List</h4>
<ul class="list-unstyled">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
</ul>
<h4>Example of Inline List</h4>
<ul class="list-inline">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
  <li>Item 4</li>
</ul>
<h4>Example of Definition List</h4>
<dl>
  <dt>Description 1</dt>
  <dd>Item 1</dd>
  <dt>Description 2</dt>
  <dd>Item 2</dd>
</dl>
<h4>Example of Horizontal Definition List</h4>
<dl class="dl-horizontal">
  <dt>Description 1</dt>
  <dd>Item 1</dd>
  <dt>Description 2</dt>
  <dd>Item 2</dd>
</dl>

<!-- Code -->
<p><code>&lt;header&gt;</code> is wrapped as an inline element.</p>
<p>To display code as a standalone block element use &lt;pre&gt; tag as:
<pre>
   &lt;article&gt;
      &lt;h1&gt;Article Heading&lt;/h1&gt;
   &lt;/article&gt;
</pre>

<!-- Table -->
<table class="table table-striped table-bordered table-hover">
   <caption>Striped Table Layout</caption>
   <thead>
      <tr>
         <th>Name</th>
         <th>City</th>
         <th>Pincode</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>Tanmay</td>
         <td>Bangalore</td>
         <td>560001</td>
      </tr>
      <tr>
         <td>Sachin</td>
         <td>Mumbai</td>
         <td>400003</td>
      </tr>
      <tr>
         <td>Uma</td>
         <td>Pune</td>
         <td>411027</td>
      </tr>
   </tbody>
</table>

<!-- Forms -->
<form role="form">
   <div class="form-group">
      <label for="name">Name</label>
      <input type="text" class="form-control" id="name" 
         placeholder="Enter Name">
   </div>
   <div class="form-group">
      <label for="inputfile">File input</label>
      <input type="file" id="inputfile">
      <p class="help-block">Example block-level help text here.</p>
   </div>
   <div class="checkbox">
      <label>
      <input type="checkbox"> Check me out
      </label>
   </div>
   <button type="submit" class="btn btn-default">Submit</button>
</form>


<!-- Buttons -->
<!-- Standard button -->
<button type="button" class="btn btn-default">Default Button</button>

<!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
<button type="button" class="btn btn-primary">Primary Button</button>

<!-- Indicates a successful or positive action -->
<button type="button" class="btn btn-success">Success Button</button>

<!-- Contextual button for informational alert messages -->
<button type="button" class="btn btn-info">Info Button</button>

<!-- Indicates caution should be taken with this action -->
<button type="button" class="btn btn-warning">Warning Button</button>

<!-- Indicates a dangerous or potentially negative action -->
<button type="button" class="btn btn-danger">Danger Button</button>

<!-- Deemphasize a button by making it look like a link while maintaining button behavior -->
<button type="button" class="btn btn-link">Link Button</button>


<!-- Images -->
<img src="/bootstrap/images/download.png" 
   class="img-rounded">
<img src="/bootstrap/images/download.png" 
   class="img-circle">
<img src="/bootstrap/images/download.png" 
   class="img-thumbnail">
```

Also this just the tip, there many layout components like: Glyphicons, Dropdowns, Button Groups, Button Dropdowns, Input Groups, Navigation Elements, Navbar, Breadcrumb, Pagination, Labels, Badges, Jumbotron, Page Header, Thumbnails, Alerts, Progress Bars, Media Object, List Group, Panels, Wells which can be found in the tutorial itself.

Also there online Bootstrap snippet library: [Bootsnipp](http://bootsnipp.com/) and themes showcase: [Bootswatch](http://bootswatch.com/).

## Reference

1. [FreeCodeCamp](http://freecodecamp.com/)
2. [Font Awesome](https://fortawesome.github.io/Font-Awesome/)
3. [A Single Div](http://a.singlediv.com/)
4. [Animated](https://daneden.github.io/animate.css/)
5. [Bootstrap](http://getbootstrap.com/)
6. [TutorialsPoint - Bootstrap](http://www.tutorialspoint.com/bootstrap/index.htm)
7. [Bootsnipp](http://bootsnipp.com/)
8. [Bootswatch](http://bootswatch.com/)
