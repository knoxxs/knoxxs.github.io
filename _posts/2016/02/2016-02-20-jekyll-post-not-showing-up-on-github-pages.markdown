---
layout: post
title:  Jekyll post not showing up on github pages
date:   2016-02-20 15:46:00
categories: programming blog jekyll
tags: github-pages jekyll bug
---

In start of Feb whenever I add any new post to github. It was not showing up on my blog. This post is about the issue behind missing jekyll posts.

## relative_permalinks

It all stated when I pushed one post on 2nd Feb. I got a failure email from github:

>The page build failed with the following error:
>
>Your site is using the `relative_permalinks` configuration option. This setting is deprecated as has been removed from the latest version of Jekyll. To ensure your site continues to build as expected, remove the option from your site's configuration and update any post or page permalinks to be absolute to the site root, not the parent folder. For more information, see [https://help.github.com/articles/removing-relative-permalinks](https://help.github.com/articles/removing-relative-permalinks).
>
>If you have any questions you can contact us by replying to this email.

I solved this by removing the `relative_permalink` from `_config.yml`.

## Missing post issue

For this I tried to take help of github support and they helped. Let me paste the email itself. It will explain the issue to:

```
> Yes, http://knoxxs.github.io/startup/2016/02/05/startup-weekend-pitch/ page showed up two days late.
> Then, http://knoxxs.github.io/programming/python/2016/02/09/python-logging/ I pushed
> yesterday and showed within last hour.
> Then,
> https://github.com/knoxxs/knoxxs.github.io/blob/master/_posts/2016/02/2016-02-10-python-decorators.markdown
> I posted two hours ago and it is still not showed up.
> I am not sure whats happening, but there is some delay/issue for sure.

Thanks for the extra info. Perhaps this is the issue:

http://jekyllrb.com/docs/upgrading/2-to-3/#all-my-posts-are-gone-whered-they-go
```

## rouge the highlighter

I also received the following warning in email:

>You are attempting to use the 'pygments' highlighter, which is currently unsupported on GitHub Pages. Your site will use 'rouge' for highlighting instead. To suppress this warning, change the 'highlighter' value to 'rouge' in your '_config.yml'. For more information, see [https://help.github.com/articles/page-build-failed-config-file-error/#fixing-highlighting-errors](https://help.github.com/articles/page-build-failed-config-file-error/#fixing-highlighting-errors).

To solve this I first changed the `highlighter` in `_config.yml` to `rouge`. On running the server I got the following error:
  
```
Dependency Error:  Yikes! It looks like you don't have rouge or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- rouge' If you run into trouble, you can find helpful resources at http://jekyllrb.com/help/! 
  Conversion error: Jekyll::Converters::Markdown encountered an error converting '_posts/2015/04/2015-04-19-mongo-internal.markdown/#excerpt'.
  Conversion error: rouge
             ERROR: YOUR SITE COULD NOT BE BUILT:
                    ------------------------------------
                    rouge
```

To solve this I updated the `github-pages` gem by running `bundle update` command in my github.io project root.

Before update:

```
$ github-pages versions
+-----------------------+---------+
| Gem                   | Version |
+-----------------------+---------+
| jekyll                | 2.4.0   |
| jekyll-coffeescript   | 1.0.1   |
| jekyll-sass-converter | 1.3.0   |
| kramdown              | 1.5.0   |
| maruku                | 0.7.0   |
| rdiscount             | 2.1.7   |
| redcarpet             | 3.3.2   |
| RedCloth              | 4.2.9   |
| liquid                | 2.6.2   |
| pygments.rb           | 0.6.3   |
| jemoji                | 0.5.0   |
| jekyll-mentions       | 0.2.1   |
| jekyll-redirect-from  | 0.8.0   |
| jekyll-sitemap        | 0.8.1   |
| jekyll-feed           | 0.3.1   |
+-----------------------+---------+
```


After update:

```
$ github-pages versions
+---------------------------+---------+
| Gem                       | Version |
+---------------------------+---------+
| jekyll                    | 3.0.3   |
| jekyll-sass-converter     | 1.3.0   |
| jekyll-textile-converter  | 0.1.0   |
| kramdown                  | 1.9.0   |
| rdiscount                 | 2.1.8   |
| redcarpet                 | 3.3.3   |
| RedCloth                  | 4.2.9   |
| liquid                    | 3.0.6   |
| rouge                     | 1.10.1  |
| jemoji                    | 0.5.1   |
| jekyll-mentions           | 1.0.1   |
| jekyll-redirect-from      | 0.9.1   |
| jekyll-sitemap            | 0.10.0  |
| jekyll-feed               | 0.4.0   |
| jekyll-gist               | 1.4.0   |
| jekyll-paginate           | 1.1.0   |
| github-pages-health-check | 1.0.1   |
| jekyll-coffeescript       | 1.0.1   |
| jekyll-seo-tag            | 1.0.0   |
+---------------------------+---------+
```

And it worked. 


## Markdown engine warning

I am still getting following warning:

```
The page build completed successfully, but returned the following warning:

You are currently using the 'redcarpet' Markdown engine, which will not be supported on GitHub Pages after May 1st. At that time, your site will use 'kramdown' for markdown rendering instead. To suppress this warning, remove the 'markdown' setting in your site's '_config.yml' file and confirm your site renders as expected. For more information, see https://help.github.com/articles/updating-your-markdown-processor-to-kramdown.
```

To solve this I added following to `_config.yml` and removed `redcarpet`:

```
markdown:         kramdown
kramdown:
  input: GFM
  hard_wrap: false
```

Finally it solved everything.
