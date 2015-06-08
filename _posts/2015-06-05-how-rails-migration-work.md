
* Rails migration I feel is a great tool to manage your database columns, It always work like charm and make my life very comfortable while managing the schema, adding removing columns in tables or any other type of modifications in database schema.

* Am I going to explain how the migrations work noooo Its already there in Rails guide go check it here:- http://edgeguides.rubyonrails.org/active_record_migrations.html 
 
* But the thing that excites me is How Rails manage schema, e.g:- I want to add user_name column in my users table.

* I run rails g migration add_user_name_to_users name and it creates file something like:-

```
class AddUserNameToUsers < ActiveRecord::Migration
  def change
    add_column :users, :user_name, :string    
  end
end
```

* Its just you run some command at terminal it generated a file and on running rake db:migrate it create all those columns in your database.

* I already had 10 extra files in my ra
