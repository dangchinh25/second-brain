# Active Records
- Active Record is the M in the MVC model
- It is an implementation of the Active Record pattern which itself is a description of an Object Relational Mapping system (ORM)
- Active Record gives us several mechanism
	- Represent models and their data
	- Represent associations between these models
	- Represent inheritance hierarchies through related models
	- Validate models before they get persisted to the database
	- Perform database operation in an object-oriented fashion

## Naming Conventions
- Active Record uses some naming conventions to find out how the mapping between models and database tables should be created.
- Rails will pluralize the class names to find the respective database table
- When using class names composed of tow or more word, the model class name should follow the Ruby conventions, using CamelCase form
	- Model class: Singular with the first letter of each word capitalized (e.g BookClub)
	- Database table: Plural with underscores separating words (e.g book_clubs)

## Creating Active Record Models
- To create Active Record models, subclass the `ApplicationRecord` class
![](https://i.imgur.com/oHC3Flf.png)

### Create
- Active Record objects can be created from a hash, a block, or have their attributes manually set after creation.
	- The `new` method will return a new object and has to call `save` to save to database
	- The `create` method will return the object and save it to database
![](https://i.imgur.com/QV2T80y.png)

![](https://i.imgur.com/dmuuKRp.png)

## Validation
- Active Record allows you to validate the state of a model before it gets written into the database
- The methods `save` and `update` take into account when running
	- return `false` when the validation fails and they don't actually perform any operations on the database
	- have a bang counterpart (`save!` and `update`) which are stricter in that they raise the exception `ActiveRecord::RecordInvalid` if validation fails
![](https://i.imgur.com/zOpARfd.png)

# Active Models
- A library containing various modules used in developing classes that need some features present on Active Record
- Some features already included in `ActiveRecord::Base` so we can immediately use like this
![](https://i.imgur.com/NH3LTUM.png)
- There are cases where classes in Rails require model-like features, but they are not tied to any table in a database => `include` in your class 
- E.g: we have a `Person` class with `name` and `id` attributes. We want to add validations to these attributes.
![](https://i.imgur.com/TD6nmSj.png)

- Ref: https://api.rubyonrails.org/classes/ActiveModel.html