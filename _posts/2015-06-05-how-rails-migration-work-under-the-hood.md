---
layout: post
title: Magical Rails migrations
---
###Rails migration is one of the best tools provided in Rails that helps you to handle database very efficiently. Here I am gonna add some internal stuff that Rails do under the hood.

* Rails migration I feel is a great tool to manage your db schema, It always work like charm and make my life very comfortable to deal with db.

* Am I going to explain how the migrations work noooo Its already there in Rails guide go check it here:- [Rails Migration](http://edgeguides.rubyonrails.org/active_record_migrations.html)
 
* But the thing that excites me is How Rails manage schema, e.g:- I want to add user_name column in my users table.

* I run rails g migration `add_user_name_to_users` name and it creates file with some content similar to :-

    ```
    class AddUserNameToUsers < ActiveRecord::Migration
      def change
        add_column :users, :user_name, :string    
      end
    end
    ```


##Scratching Head:

* You ran some command in terminal it generated a file and on running `rake db:migrate` it create all those columns in your database and It feels like magic, no?

* I already had 10 extra files in my `db/migrate` folder, how does rails exactly know its just this file that need to run.

* I have seen other developers working on other framworks and its a pain in the ass to handle db changes, they used to take db dumps, share it within team and in Rails it works without any effort this thing increased the curiosity to many folds.

##Digging down:

* When ever you try to create some migration file it take the timestamp and put that in filename e.g:- if this is filename `20150714204601_add_username_to_users` `20150714204601` is timestamp.

* When tried digging down the system I came across there is one more interesting thing that rails created a table named `schema_migration` and it is having timestamp of all the files of migration.

##Eureka moment :p

* And that eureka moment arrived when I find out that basically what Rails does is it keeps the file timestamp in this `schema_migration` and whenever we run `rake db:migrate` it checks if there is any other file whose time stamp is not there in the table it pass that file to parser.

* Then Migration class parses the content and run sql queries accordingly for those files only.

* This is one of those things that I felt Rails handles greatly, well I came across this when Migration stopped working for me :p, on production as my db was very big for deployment script to migrate and I had to run all these queries directly :p



