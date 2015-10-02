---
layout: post
title:  "Organization github pages and ember"
date:   2015-10-02 00:36:00
categories: programming gh-pages ember web
tags: ember-cli ember-cli-github-pages git github pages 
---

Today we planned to host our website on github-pages as a static website. We decided to used `ember` web framework to develop this website. In case of organisation/user github pages, github reads the website source from the `master` branch not the `gh-pages` branch. This causes an issue: where to keep the ember specific code. As in ember the output code is in `dist/` directory. Github needs the code inside the `dist` directory to be present in the root directory of the project. So the solution is:

1. Create two branches:
    1. `ember`: For ember code
    2. `master`: For the final website code.
2. `ember` will be your main branch. Do all the work there.
3. Then do `ember build --environment=production`.
4. Copy all the files in the `/dist` folder to the root of your `master` branch.
5. Commit and push.

There is a `ember-cli` plugin which can do all these things for you: [ember-cli-github-pages](https://github.com/poetic/ember-cli-github-pages). But the readme of this plugin explains everything for Project github pages not for organisation github pages. But after spending some time I finally made it work for the organisation pages. Here are the steps:

1. Create new emebr project `ember new myBlog`. Here `myBlog` is the name of the project.
2. `cd` to myBlog and install ember-cli-github-pages:
    
    ```bash
    cd myBlog && ember install ember-cli-github-pages
    ```
3. Commit the changes: 
    
    ```bash
    git add -A && git commit -m "Added ember-cli-github-pages addon  https://github.com/poetic/ember-cli-github-pages"
    ```
4. Create new branch named `ember` which will store all the ember related code: `git checkout -b ember`
5. Run the following command as mentioned [above](https://github.com/poetic/ember-cli-github-pages#installation--setup):
    
    ```bash
    git checkout master && rm -rf `ls -a | grep -vE '\.gitignore|\.git|node_modules|bower_components|(^[.]{1,2}/?$)'` && git add -A && git commit -m "initialises gh-pages(in case of organisation master) commit"
    ```
6. Switch back to ember branch:
    
    ```bash
    git checkout ember
    ```
7. Build the site using ember-cli-github-pages:
    
    ```bash
    ember github-pages:commit --branch master --message "adds base site"
    ```
8. Create new Org/User repo on Github and add the origin:
    
    ```bash
    git remote add origin https://github.com/knoxxs/knoxxs.github.io.git
    ```
    Here _knoxxs_ is my username.
9. Push the master branch:
    
    ```bash
    git push -u origin master
    ```
10. Open `http://knoxxs.github.io/`.

I already made a [pull request](https://github.com/poetic/ember-cli-github-pages/pull/36) to include these steps in the plugin readme.
 
## References

1. [ember-cli-github-pages](https://github.com/poetic/ember-cli-github-pages)