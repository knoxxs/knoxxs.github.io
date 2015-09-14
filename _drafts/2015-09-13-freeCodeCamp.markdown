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

Bonus: Awesome things people have ,ade with pseudo elements : [A Single Div](http://a.singlediv.com/)

## Animated
[Animated](https://daneden.github.io/animate.css/) is a css lib which help you use the power css animations.

## Bootstrap
[Bootstrap](http://getbootstrap.com/) is the most popular HTML, CSS, and JS framework for developing responsive, mobile first projects on the web.

Bootstrap is too big. They don't have any official reference guide. The closes tutorial to the reference guide I was able to find was [TutorialsPoint - Bootstrap](http://www.tutorialspoint.com/bootstrap/index.htm)

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
