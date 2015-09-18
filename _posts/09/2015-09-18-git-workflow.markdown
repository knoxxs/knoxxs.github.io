---
layout: post
title:  "Git Workflow - Adding more structure to git"
date:   2015-09-18 15:14:00
categories: programming git
tags: git workflow git-flow centralised featured-branch forking topic-branch rebase collaboration version-control github-flow
---

I have tried reading the workflow one should follow for there git repo. But every time I left in between with many doubts in my mind. Now I have to do it as while developing [Supertext](http://supertextnow.com/) platform we are at a stage where we have to start following a perfect workflow. As many people are working on the same repo and also there are production, qa, and local version of the code. So lets do it.

Workflow is just some set of conventions that one can follow to have smooth git journey mainly when there are are multiple people contributing to the same project. Different flows depend upon different team organisation and different contribution model.

## Centralized Workflow
I initially planned to read the [atlassian guide on comparing the different workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/) & some more guides on the same topic and then write my views here. But after reading the centralized workflow I feel like there is no need of reading any other guide or writing anything here in my way as atlassian people have explained it so well. So I suggest to only read that.

Though I am writing some bullet points which will help me remember things:

1. Like Subversion, the Centralized Workflow uses a central repository to serve as the single point-of-entry for all changes to the project.
2. Developers start by cloning the central repository. In their own local copies of the project, they edit files and commit changes locally—they’re completely isolated from the central repository.
3. To publish changes to the official project, developers `push` their local `master` branch to the central repository.
4. If a developer’s local commits diverge from the central repository, Git will refuse to push their changes because this would overwrite official commits.
5. Then the developer has to fetch the changes from the central repository and have to merge(`git pull origin master`)/rebase(`git pull --rebase origin master`) them.
    - In case of merge you would wind up with a superfluous *merge commit* every time someone needed to synchronize with the central repository.
    - For this workflow, it’s always better to rebase instead of generating a merge commit.

####  Why rebase is better than merge

1. If local changes directly conflict with upstream commits, Git will pause the rebasing process and give you a chance to manually resolve the conflicts.
2. Rebasing works by transferring each local commit to the updated master branch one at a time. This means that you catch merge conflicts on a commit-by-commit basis rather than resolving all of them in one massive merge commit..

## Feature Branch Workflow

Following are some the points which captures the gist of this workflow:

1. The Feature Branch Workflow still uses a central repository, and master still represents the official project history.
2. But, instead of committing directly on their local `master` branch, developers create a new branch every time they start work on a new feature. This encapsulation makes it easy for multiple developers to work on a particular feature without disturbing the main codebase.
3. In addition, feature branches can (and should) be pushed to the central repository.
4. Once someone completes a feature, they don’t immediately merge it into `master`. Instead, they push the feature branch to the central server and file a `pull request` asking to `merge` their additions into `master`. This gives other developers an opportunity to review code.
5. Code review is a major benefit of pull requests, but they’re actually designed to be a generic way to talk about code. This means that they can also be used much earlier in the development process. For example, if a developer needs help with a particular feature, all they have to do is file a pull request.
6. Once a pull request is accepted, the actual act of publishing a feature is much the same as in the Centralized Workflow.

    ```bash
    # Make sure your local master is synchronized with the upstream master
    git checkout master
    git pull

    #Merge the feature branch into master
    git pull origin marys-feature

    #push the updated master back to the central repository
    git push
    ```
7. This process often results in a merge commit. Some developers like this because it’s like a symbolic joining of the feature with the rest of the code base. But, if you’re partial to a linear history, it’s possible to rebase the feature onto the tip of master before executing the merge, resulting in a fast-forward merge.

## Gitflow Workflow

![Gitflow architecture]({{ site.url }}assets/2015/09/git-workflow/giflow.png)

The Gitflow Workflow defines a strict branching model designed around the project release. While somewhat more complicated than the Feature Branch Workflow, this provides a robust framework for managing larger projects. This workflow doesn’t add any new concepts or commands beyond what’s required for the Feature Branch Workflow. Instead, it assigns very specific roles to different branches and defines how and when they should interact. In addition to feature branches, it uses individual branches for preparing, maintaining, and recording releases.

Following are some the points which captures the gist of this workflow:

1. The Gitflow Workflow still uses a central repository as the communication hub for all developers.
2. Instead of a single `master` branch, this workflow uses two branches to record the history of the project: `master & develop`
    - The `master` branch stores the official release history
    - The `develop` branch serves as an integration branch for features
    - It's also convenient to tag all commits in the `master` branch with a version number.
3. Each new feature should reside in its own feature/topic branch. But, instead of branching off of `master`, feature branches use `develop` as their parent branch. Features should never interact directly with `master`.
4. Once `develop` has acquired enough features for a release, you fork a `release` (conventional name: `release-* or release/*`) branch off of `develop`.
    - Creating this branch starts the next release cycle, so no new features can be added after this point
    - Only bug fixes, documentation generation, and other release-oriented tasks should go in this branch.
    - Once it's ready to ship, the `release` gets merged into `master` and tagged with a version number.
    - In addition, it should be merged back into `develop`, which may have progressed since the release was initiated.
5. Maintenance or `hotfix` branches are used to quickly patch production releases. This is the only branch that should fork directly off of master. As soon as the fix is complete, it should be merged into both `master` and `develop` (or the current `release` branch), and `master` should be tagged with an updated version number.

Atlassian provide a very brief example for [this workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

## Forking Workflow
The Forking Workflow is fundamentally different than the other . Instead of using a single server-side repository to act as the “central” codebase, it gives every developer a server-side repository. The main advantage of the Forking Workflow is that contributions can be integrated without the need for everybody to push to a single central repository. Developers push to their own server-side repositories, and only the project maintainer can push to the official repository. This allows the maintainer to accept commits from any developer without giving them write access to the official codebase.

Following are some the points which captures the gist of this workflow:

1. As in the other Git workflows, the Forking Workflow begins with an official public repository stored on a server.
2. When a new developer wants to start working on the project, they do not directly clone the official repository. Instead they do the following:
    - Fork the official repository to create a copy of it on the server. This new copy serves as their personal public repository.
    - After they have created their server-side copy, the developer performs a git clone to get a copy of it onto their local machine. This serves as their private development environment, just like in the other workflows.
3. When they're ready to publish a local commit, they push the commit to their own public repository—not the official one.
4. Then, they file a pull request with the main repository, which lets the project maintainer know that an update is ready to be integrated.
5. To integrate the feature into the official codebase, the maintainer does the following:
    - Pulls the contributor’s changes into their local repository
    - Checks to make sure it doesn't break the project
    - Merges it into his local master branch
    - Pushes the `master` branch to the official repository on the server.

## Github Workflow

Issues in Git-Flow:

1. It’s more complicated than most developers and development teams actually require.
2. The git-flow process is designed largely around the _release_. For those who don't really have _releases_ because they deploy to production every day - often several times a day. May be using CI.

GitHub Flow is a lightweight, branch-based workflow that supports teams and projects where deployments are made regularly. Following are some the points which captures the gist of this workflow:

1. Create a feature branch from `master`.
2. Work on it as one does in Feature-Workflow
3. Open a pull request if you need help in-between or when you are done.
4. Discuss and review your code
5. **Deploy**: Once your pull request has been reviewed and the branch passes your tests, you can deploy your changes to verify them in production. If your branch causes issues, you can roll it back by deploying the existing `master` into production.
6. Merge

## References

1. [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/)
2. [Understanding the GitHub Flow](https://guides.github.com/introduction/flow/index.html)
3. [GitHub Flow - Scott Chacon](http://scottchacon.com/2011/08/31/github-flow.html)