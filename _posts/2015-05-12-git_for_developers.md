---
layout: post
title: Awesome Git
---

### Git tips for Developers.

I use Git all the time, its awesome tool for the versions control.

Most of the developers I came across don't know much other than commiting, branching, merging etc. I would love to add couple of other commands that helped me a lot during managing code base, hopefully will help you too.

* <a href="http://git-scm.com/docs/git-blame" target="_blank">Git blame</a>(Blame it on somebody, life will be very easy :p ):-


    * Let's take an example you have some massive code base and 10 programmers are working on your app. There is some broken part of code and you want to figure out who wrote this, try <a href="http://git-scm.com/docs/git-blame" target="_blank">Git blame</a>

        ```
        git blame my_awesome_file
        ```

    * It will give your something like:-

        ```
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  1) ---
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  2) layout: post
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  3) title: Rails and Git hooks
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  4) ---
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  5)
        4fd0e67e (Peeyush Singla 2015-04-25 01:07:11 +0530  6) ### Automate your repeated task like **bundle install** and **rake db:migrate** on every pull.
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  7)
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  8) ###Procedure:
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530  9)
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530 10) * Enter into hooks directory in your projects.
        771a0506 (Peeyush Singla 2015-04-18 00:03:02 +0530 11)
        c002c48b (Peeyush Singla 2015-04-17 23:58:30 +0530 12) ```
        b4eca87b (Peeyush Singla 2015-04-17 23:44:53 +0530 13) cd myawesomeproject/.git/hooks
        ```

    * It will show commit history, commiter name for every line of code, but I used to have 1000 lines in my file and git blame this way don't work for me. I used to need to blame only the small part of whole file. Don't worry there is a solution:-

        ```
        git blame -L 1,10 my_awesome_file
        ```

    * Here -L is lines and 1 is starting and 10 is end line. Now it will show all the details of only those lines and saved a lot of time for me to check whole file.

    * Other thing is I use Sublime text for development purpose and here is the awesome [Git package](https://github.com/kemayo/sublime-text-git) that provide it inside the sublime.


* <a href="http://git-scm.com/docs/git-log" target="_blank">Git log</a> (Logging deails the Git way):-

    * Git log by default show the commit hash, commit message and commiter but I used to check what was the content that were the changes in that commit:-

        ```
        git log -p -number_of_commits
        ```

    * I used to check what are changes in specific file w.r.t commits because I need to check what was the timline of changes for that specific file, and git log is awesome tool for it.

        ```
        git log path_of_file
        ```

    * It will print all the commits in that file only now you need to check the changes in that file as w.r.t commits.

        ```
        git log -p path_of_file
        ```

* <a href="http://git-scm.com/docs/git-stash" target="_blank">Git Stash</a>(Smash the code with git stash):-

    * Git stash is another very useful command, Let's assume the case when I have done couple of changes in file and all of sudden I need to work on some hot fix for deployment.

    * First step:-

        ```
        git stash
        ```

    * Git stash will add my changes to stash and my working directroy will be pointing to HEAD again.

    * Now I did all the changes required for hot fix and deployed, now I need the code that I was working on back. I will do:-

        ```
        git stash pop
        ```

    I will get the code I was working on back in my directory.








