---
layout: post
title:      "Rails cheat sheet!"
date:       2019-01-21 15:06:03 +0000
permalink:  rails_cheat_sheet
---


1. Creating a migration - Rails is smart, you will be able to generate a table with just this in your console:
"rails generate migration coupons"
"rails generate migration students"

This will create a table faster than in Sinatra where you had to type this in your console:

"rake db:create_migration NAME=create_dogs"

This essentially does the same thing. 

2. Naming conventions - You need to keep the file names very consistent with each other so they know how to communicate.  For example you need to keep these in constant format:

 - make sure the filename for the controller is in snake case: "coupons_controller.rb"
 
 - make sure that the controller class is in camel case: "CouponsController" 
          class CouponsController < ActionController::Base 
					class Coupons Controller< ApplicationController 
					(note either one will be acceptable, because the ApplicationController typically inherits from the ActionController, and then you are inheriting from the ApplicationController)
					
 - make sure the file name for the model is not in plural form: "coupon.rb"
 - 
 - make sure the class name for the model is not in plural form: 
         class Coupon < ActiveRecord::Base 






Just a compilation of stuff I'm trying to keep in one place:

2.3 Path and URL Helpers
Creating a resourceful route will also expose a number of helpers to the controllers in your application. In the case of resources :photos:

1. photos_path -  returns /photos

2. new_photo_path  - returns /photos/new

3. edit_photo_path(:id)  - returns /photos/:id/edit (for instance, edit_photo_path(10) returns /photos/10/edit)

4. photo_path(:id) -  returns /photos/:id (for instance, photo_path(10) returns /photos/10)

Each of these helpers has a corresponding _url helper (such as photos_url) which returns the same path prefixed with the current host, port, and path prefix.

2.4 Defining Multiple Resources at the Same Time
If you need to create routes for more than one resource, you can save a bit of typing by defining them all with a single call to resources:

resources :photos, :books, :videos
This works exactly the same as:

resources :photos
resources :books
resources :videos


form_for = we use this because it automatically attaches to a model for us.  For example it is better to use form_for when creating a profile.  Essentially, it allows for a cleaner way to create a form, it also automatically assigns key values to any model attribtutes, by merely typing in the attribute.  

Also, form_for is bound to the model you use it with, so in the case of params earlier, you would enter your key values through the html like this: 

<%= form_tag post_path(@post), method: "put" do %>
<label> Post Title: </label></br>
<% text_field_tag :title, @post.title %>

This would create a params: {title: whatever}

But with form_for, you have a key of "post" in your params because they are now bound together.  So it would be something like : 

params: 
=> {"utf8"=>"✓",
 "post"=>{"title"=>"My post title", "description"=>"My post description"},
 "commit"=>"Create Post",
 "controller"=>"posts",
 "action"=>"create"}

Notice that now there is a key of "post" and this is a hash where we can store our model attributes.  Remember, if using form_for, then we need to think about our "update" method in our controller differently: 

Because form_for is bound directly with the Post model, we need to pass the model name into the Active Record update method in the controller. Let's change @post.update(title: params[:title], description: params[:description]) to:
@post.update(params.require(:post))

as explained above, we need to have params.require(:post) because now our attributes are nested inside of our "post" key.  This ties in to our Strong Params restriction: 

Quick summary: 
form_tag is more like a search engine parameter, doens't know anything about what you're trying to do, you have to specify whether it's a post request or a patch request.  It's better thought of as a search engine form. Best not used for CRUD.  

form_for make assumptions and creates a FormBuilder object.  You can use that to create your form elements.  form_for handles the retrieval of values from your object model and will also try to route the form to the appropriate action specified in the controller.  It is best used for CRUD as it would have an activerrecord set up.  



Strong Params  = 

Essentially augment the params so that we REQUIRE and PERMIT certain parameters for our methods.  We bottle neck our params by requiring whatever model that we are creating.  

First, we changed our form methods by using the "form_for"  by requiring that we have an attribute of "post".  

Then we have the added value of "pemit."  The difference is basically that you MUST have an attribute of "post", but if you are working with "permit" and you are trying to create a model, you can have one attribtue missing and there wouldn't be a problem.  But the has won't accept any other keys except what you "permit".  So there is a bit of leniency in terms of the creation of a model.  

Strong Params allows us to really narrow down what we accept in our forms.  We make this change by creating a "private" section of our controller, and we define our post_params requirements there: 

	def post_params(*args)
		params.require(:post).permit(*args)
	end
	
	We have the permit(*args) to accept those attribtues we want.  Sometimes we may want just one to be edited (i.e. you can edit your name, but you can't edit your bank balance) 
	
	Note update: 
	
	Reminder, strong params will help isolate what you are requiring in your models.  
	
	` {"song"=>
  {"title"=>"Talisman",
   "artist_name"=>"Air",
   "release_year"=>"2007",
   "released"=>true,
   "genre"=>"Post-Rock"},
 "controller"=>"songs",
 "action"=>"create"}`
 
 In this params, we see that there are 3 keys, "song", "controller", and "action".  "song" has a nested hash, where we want those values to create our model. 
 
 So our code to reinforce strong params would be: 
 
 def song_params 
    params.require(:songs).permit(:title, :artist_name, :release_year, :released, :genre)
end 
	
	RAILS GENERATORS:
	
	https://learn.co/tracks/full-stack-web-development-v6/rails/crud-with-rails/rails-generators
	
	A lot of good info on how to generate a controller, a model, a table, really quickly with one line of code, it also breaks down what is a good generator and it's purposes are vs. the drawbacks on using particular generators. 
	Final word on generators - skip the scaffold generator, it is too much bloat, they recommend the best is "rails g resource." 
	
	
	Quick links:
	
	Here's where we learn about URL Helpers, specifically helps with formatting the "link_to" issue:
	https://learn.co/tracks/full-stack-web-development-v6/rails/intro-to-actionview/rails-url-helpers
	
	
	VALIDATION HELPERS: 
	
	VALIDATION: 
	
	The following methods trigger validations, and will save the object to the database only if the object is valid:

create
create!
save
save!
update
update_attributes
update_attributes!
The bang versions (e.g. save!) raise an exception if the record is invalid. The non-bang versions don’t: save and update_attributes return false, create and update just return the object/s.

If you are updating something in your controller, merely setting the "@posts.update" method, will automatically trigger validations.  In other words, it will hit our "validate" methods in our Model, (Post) and cross check all the fields to see if they are valid.  If they are, then they will update it. 

```
def update
    if @post.valid?
      @post.update(post_params)
    redirect_to post_path(@post)
  else
    render :edit
    end
  end
```

In this method of update, you don't need to hit "@post.valid?", as the @post.update(post_params) will automaticlaly trigger validations.  

trying to submit invalid 
