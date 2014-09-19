# Rails Nested Fields Params

To access the nested fields from params do the following:

``` ruby
params[:order][:items_attributes].values.each do |item|
  item[:type_id]
end if params[:order] and params[:order][:items_attributes]
```

Above solution will work ONLY if you have declared the correct associations and accepts_nested_attributes_for.

```ruby
class Order < ActiveRecord::Base
  has_many :items
  accepts_nested_attributes_for :items, :allow_destroy => true
end

class Item < ActiveRecord::Base
  belongs_to :order
end

```

# Ruby each Iterator

The each iterator returns all the elements of an array or a hash.

Syntax:

``` ruby
collection.each do |variable|
   code
end
```

Executes code for each element in collection. Here, collection could be an array or a ruby hash.

Example:

``` ruby
#!/usr/bin/ruby

ary = [1,2,3,4,5]
ary.each do |i|
   puts i
end

```

This will produce the following result:

``` bash
1
2
3
4
5
```

You always associate the each iterator with a block. It returns each value of the array, one by one, to the block. The value is stored in the variable i and then displayed on the screen.

# Ruby collect Iterator

The collect iterator returns all the elements of a collection.

Syntax:

``` ruby
collection = collection.collect
```

The collect method need not always be associated with a block. The collect method returns the entire collection, regardless of whether it is an array or a hash.

Example:

``` ruby
#!/usr/bin/ruby

a = [1,2,3,4,5]
b = Array.new
b = a.collect
puts b
```

This will produce the following result:

``` bash
1
2
3
4
5
```

NOTE: The collect method is not the right way to do copying between arrays. There is another method called a clone, which should be used to copy one array into another array.

You normally use the collect method when you want to do something with each of the values to get the new array. For example, this code produces an array b containing 10 times each value in a.

``` ruby
#!/usr/bin/ruby

a = [1,2,3,4,5]
b = a.collect{|x| 10*x}
puts b
```

This will produce the following result:

``` bash
	10
	20
	30
	40
	50
```

# find\_or\_create_by(attributes, &block)

Finds the first record with the given attributes, or creates a record with the attributes if one is not found:

``` ruby
# Find the first user named "PenÃ©lope" or create a new one.
User.find_or_create_by(first_name: 'PenÃ©lope')
# => #<User id: 1, first_name: "PenÃ©lope", last_name: nil>

# Find the first user named "PenÃ©lope" or create a new one.
# We already have one so the existing record will be returned.
User.find_or_create_by(first_name: 'PenÃ©lope')
# => #<User id: 1, first_name: "PenÃ©lope", last_name: nil>

# Find the first user named "Scarlett" or create a new one with
# a particular last name.
User.create_with(last_name: 'Johansson').find_or_create_by(first_name: 'Scarlett')
# => #<User id: 2, first_name: "Scarlett", last_name: "Johansson">
```

This method accepts a block, which is passed down to create. The last example above can be alternatively written this way:

``` ruby
# Find the first user named "Scarlett" or create a new one with a
# different last name.
User.find_or_create_by(first_name: 'Scarlett') do |user|
  user.last_name = 'Johansson'
end
# => #<User id: 2, first_name: "Scarlett", last_name: "Johansson">
```

This method always returns a record, but if creation was attempted and failed due to validation errors it won’t be persisted, you get what create returns in such situation.

Please note *this method is not atomic*, it runs first a SELECT, and if there are no results an INSERT is attempted. If there are other threads or processes there is a race condition between both calls and it could be the case that you end up with two similar records.

Whether that is a problem or not depends on the logic of the application, but in the particular case in which rows have a UNIQUE constraint an exception may be raised, just retry:

``` ruby
begin
  CreditAccount.find_or_create_by(user_id: user.id)
rescue ActiveRecord::RecordNotUnique
  retry
end
```


# Trucate

``` ruby
truncate("Once upon a time in a world far far away")
# => "Once upon a time in a world..."
truncate("Once upon a time in a world far far away", :length => 17)
# => "Once upon a ti..."
truncate("Once upon a time in a world far far away", :length => 17, :separator => ' ')
# => "Once upon a..."
truncate("And they found that many people were sleeping better.", :length => 25, :omission => '... (continued)')
# => "And they f... (continued)"
```

# Active Record Order

Allows to specify an order attribute:

``` ruby
User.order('name')
=> SELECT "users".* FROM "users" ORDER BY name

User.order('name DESC')
=> SELECT "users".* FROM "users" ORDER BY name DESC

User.order('name DESC, email')
=> SELECT "users".* FROM "users" ORDER BY name DESC, email

User.order(:name)
=> SELECT "users".* FROM "users" ORDER BY "users"."name" ASC

User.order(email: :desc)
=> SELECT "users".* FROM "users" ORDER BY "users"."email" DESC

User.order(:name, email: :desc)
=> SELECT "users".* FROM "users" ORDER BY "users"."name" ASC, "users"."email" DESC
```


# Active Record assign_attributes

```assign_attributes(new_attributes, options = {}) public```

Allows you to set all the attributes for a particular mass-assignment security role by passing in a hash of attributes with keys matching the attribute names (which again matches the column names) and the role name using the :as option.

To bypass mass-assignment security you can use the :without_protection => true option.

``` ruby
class User < ActiveRecord::Base
  attr_accessible :name
  attr_accessible :name, :is_admin, :as => :admin
end

user = User.new
user.assign_attributes({ :name => 'Josh', :is_admin => true })
user.name       # => "Josh"
user.is_admin?  # => false

user = User.new
user.assign_attributes({ :name => 'Josh', :is_admin => true }, :as => :admin)
user.name       # => "Josh"
user.is_admin?  # => true

user = User.new
user.assign_attributes({ :name => 'Josh', :is_admin => true }, :without_protection => true)
user.name       # => "Josh"
user.is_admin?  # => true
```

# Fully Customizing Devise Routes

Devise is a full-featured authentication and account management framework for Ruby on Rails. It has many options, but the default configuration is definitely encouraged.

There are some basic URL (path) options that allow you to modify the basics of the routes that Devise supplies for all of its features, but they are very minimal.

``` ruby
devise_for :users, path_names: {
  sign_in: 'login',
  sign_out: 'logout',
  password: 'reset'
}
```

This means that Devise will use /login and /logout, but when it comes to doing password resets, confirmations, registrations, etc., it’s going to tack on the default Rails CRUD actions ```/reset/new```, ```/reset/edit```, ```/confirmations/new``` etc.

I can’t abide by those ugly URLs and want full customization ability for every single route. A brief aside: I’m using the model Person in my latest app instead of the ubiquitous User. I find the former to be more humanizing.

Given that, in order to customize every single route across the sessions, passwords, confirmations and registrations controllers for Devise, I used the following:

``` ruby
# Authentication
  devise_for :people, skip: [:sessions, :passwords, :confirmations, :registrations]
  as :person do
    # session handling
    get     '/login'  => 'devise/sessions#new',     as: 'new_person_session'
    post    '/login'  => 'devise/sessions#create',  as: 'person_session'
    delete  '/logout' => 'devise/sessions#destroy', as: 'destroy_person_session'

    # joining
    get   '/join' => 'devise/registrations#new',    as: 'new_person_registration'
    post  '/join' => 'devise/registrations#create', as: 'person_registration'

    scope '/account' do
      # password reset
      get   '/reset-password'        => 'devise/passwords#new',    as: 'new_person_password'
      put   '/reset-password'        => 'devise/passwords#update', as: 'person_password'
      post  '/reset-password'        => 'devise/passwords#create'
      get   '/reset-password/change' => 'devise/passwords#edit',   as: 'edit_person_password'

      # confirmation
      get   '/confirm'        => 'devise/confirmations#show',   as: 'person_confirmation'
      post  '/confirm'        => 'devise/confirmations#create'
      get   '/confirm/resend' => 'devise/confirmations#new',    as: 'new_person_confirmation'

      # settings & cancellation
      get '/cancel'   => 'devise/registrations#cancel', as: 'cancel_person_registration'
      get '/settings' => 'devise/registrations#edit',   as: 'edit_person_registration'
      put '/settings' => 'devise/registrations#update'

      # account deletion
      delete '' => 'devise/registrations#destroy'
    end
  end
```

The trick is to make sure your as named route aliases line up correctly with what Devise expects, and to ensure that you call devise_for before devise_scope (or its alias, as, like I did). You need to tell devise_for to skip the auto-creation of all of the routes for all of the controllers you’re using, then go ahead and define all of them yourself.

# Rails 4 Turbolinks Override

### First solution: disable Turbolinks

Disable turbolinks in a given page by passing a data-attribute:

``` ruby
= link_to "My rating feature", rating_feature_path,
  "data-no-turbolink" => true
```

### Second solution: create a trigger handler 

``` javascript
attachRatingHandler = ->
  $("span.star").on "click", ->
    ... code dealing with ratings ...

$(document).ready attachRatingHandler
$(document).on "page:load", attachRatingHandler

Article from [I am Pedantic](http://iampedantic.com/post/41170460234/fully-customizing-devise-routes).
```

or even better:

``` javascript
attachRatingHandler = ->
  ... code dealing with ratings ...
$ ->
  $(document).on 'click', 'span.star', attachRatingHandler
```

from [Rails 4: My First Run-in with Turbolinks](http://srbiv.github.io/2013/04/06/rails-4-my-first-run-in-with-turbolinks.html)

# Nested Model Links

* [Nested Model Form in Rails 4](http://iroller.ru/blog/2013/10/14/nested-model-form-in-rails-4/)
* [Rails Nested Forms using jQuery and SimpleForm](http://davidlesches.com/blog/rails-nested-forms-using-jquery-and-simpleform)
* [Active Record Nested Attributes](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html)
* [Complex Rails Forms with Nested Attributes](http://www.sitepoint.com/complex-rails-forms-with-nested-attributes/)
* [Rails awesome_nested_fields gem](https://github.com/lailsonbm/awesome_nested_fields)
* [Working with JavaScript in Rails](http://edgeguides.rubyonrails.org/working_with_javascript_in_rails.html)
* [accepts_nested_attributes_for](http://apidock.com/rails/ActiveRecord/NestedAttributes/ClassMethods/accepts_nested_attributes_for)
* [Working with nested forms and a many-to-many association in Rails 4](http://www.createdbypete.com/articles/working-with-nested-forms-and-a-many-to-many-association-in-rails-4/)
* [fields_for](http://apidock.com/rails/ActionView/Helpers/FormHelper/fields_for)
