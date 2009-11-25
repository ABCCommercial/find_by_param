find_by_param
=============

*find_by_param* helps you dealing with permalinks and finding objects by our
permalink value

    class Post < ActiveRecord:Base
      make_permalink :with => :title
    end

now you can do 
  
    Post.find_by_param(...)

If you have a permalink-column *find_by_param* saves the permalink there and
uses that otherwise it just uses the provided attribute.


Examples
========

Configuration
-------------

    make_permalink :with => :login
    make_permalink :with => :title, :prepend_id => true
    make_permalink :with => :name, :forbidden => %w(new edit)

Client code
-----------

    Post.create(:title => "hey ho let's go!").to_param #=> "hey-ho-lets-go" 
              # ... to_param is the method Rails calls to create the URL values

    Post.find_by_param("hey-ho-lets-go") #=> <Post>

    Post.find_by_param("is-not-there")   #=> nil
    Post.find_by_param!("is-not-there")  #=> raises ActiveRecord::RecordNotFound


Basic Documentation
===================

Options for make_permalink
--------------------------

The following options may be used, when configuring the permalink generation
with `make_permalink`.

 *   `:with` (required)

     The attribute that should be used as permalink

 *   `:field`

     The name of your permalink column. make_permalink first checks if there is 
     a column. 

 *   `:prepend_id => [true|false]`

     Do you want to prepend the ID to the permalink? for URLs like:
     `posts/123-my-post-title` - *find_by_param* uses the ID column to search.

 *   `:escape => [true|false]`

     Do you want to escape the permalink value? (strip chars like `öä?&`) -
     actually you must do that

 *   `:forbidden => [Regexp|String|Array of Strings]`

     Define which values should be forbidden. This is useful when combining user
     defined values to generate permalinks in combination with restful routing.
     **Make sure, especially in the case of a Regexp argument, that values may
     become valid by adding or incrementing a trailing integer.**


Class methods provided by *find_by_param*
---------------------------------------

The following methods are added as public methods to all classes containing a
permalink field.

 *   `find_by_param(id)`

     Look up a value by its permalink value, returns matching instance or
     `nil`, if none is found.

 *   `find_by_param!(id)`

     Look up a value by its permalink value, returns matching instance or
     raises `ActiveRecord::RecordNotFound`, if none is found.


Issues
=======

* Alex Sharp (http://github.com/ajsharp) pointed to an issue with STI. Better call make_permalink in every child class and not only in the parent class..
* write nice docs
* write nicer tests

Copyright (c) 2007 \[Michael Bumann - Railslove.com\], released under the MIT license
