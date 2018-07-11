---
layout: post
title:      "Quick Cheatsheet of Terms for Active Record and Sinatra"
date:       2018-07-11 00:00:34 +0000
permalink:  quick_cheatsheet_of_terms_for_active_record_and_sinatra
---


**ORM** - Object Relational Mapping - 

**SQL** - (Structured Query Language) standard language for manipulating databases.  You can CRUD data, tables and indexes. 

**Active Record Migrations** - This is a way to modify your database, in the familiar ruby language format.  A typical migration will look like this: 

```
class CreateProducts < ActiveRecord::Migration[5.0]

     def change 
        create_table  :products do |t| 
				t.string   :name 
				t.integer :age
		  end 
  end 
end 

```

You can add, edit, delete columns, entries.  Active Record will automatically update your changes in the schema.  

**Primary Keys **- Active Record creates primary keys by default for you.  This is just named "id" 

**Foreign Keys** - they are the keys that reference the Main or Parent Table that connect to the Sub or Child table.  Keeps data integrity.  For example - if you have a customers table and an orders table,  the foreign key of the orders table will be the primary key of the customers table.  This easy reference will solidify that relationship and will prevent a non-existent customer to be entered in the orders table, as all foreign keys only reference existing customers.  



**Active Record Associations** - associations between two models.  For example, Book class and Author class.  The most popular are "has_many" and "belongs_to".  Associations make deleting, updating, creating models much easier.  



Some tips in debugging: 
1. You can always rollback your migrations with "rake db:rollback" - this however will only rollback one migration at a time. 
2. Sometimes you will need to delete your data.  Delete the "development.sqlite" and the "test.sqlite" files, then run "rake db:migrate" to populate the data once more. 
3. Remember, that the "belongs_to" relationship will only have the name of the model (not in plural) but the "has_many" relationship needs to pluralize the table. 
4. Consider the Artist -Song-Genre.  An artist has many genres THROUGH songs.  ie.:

      Katy Perry - "song1" - rap
			                      - "song2" - pop
			                      - "song3" - ballad
			                      
			How these relationships play out, we need to make sure that the relaitonship is stated for artist as:
			
		`class Artist < ActiveRecord::Base
      has_many :songs
      has_many :genres, through: :songs
     end
`
5.  You can delete the databases, do the rake db:rollback to rollback the previous migrations, get then do rake db:migrate and have this set up a new database. 

Hope this helps in your Sinatra Section! 

