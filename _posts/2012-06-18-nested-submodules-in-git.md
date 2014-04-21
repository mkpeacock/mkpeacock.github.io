---
layout: post
title:  "Nested submodules in Git"
date:   2012-06-18 10:52:18
author: Michael
---
I'm currently working on a project which has been split into a number of distinct parts, and as a result we have a number of seperate Git repositories of code for the project. We decided to use submodules to manage these dependencies, and set it up so that the four repositories were nested, one inside the other, inside the other, inside the other.

When working with nested submodules, its a little bit of a pain to get all of the submodules checked out, and updated (assuming you want your projects to update as each dependant submodule changes). [Lewis](http://www.twitter.com/2bard) and I put our heads together and came up with this solution.

When you have cloned a project with submodules, first run this. It will intiailise and subsequently clone each of the submodules.

`git submodule update --recursive --merge --init`

When you are ready to update the code to take into account changes to submodule code, first, do a pull on your main repository.

`git pull`

Then tell git to look recursively through each submodules, ensure we are tied to a branch and not running headless (which will prevent a pull), then pull the changes.

`git submodule foreach --recursive 'git checkout master && git pull'`

Now you can make use of nested submodules in your project and have changes down the line pulled all the way up to your main project.
