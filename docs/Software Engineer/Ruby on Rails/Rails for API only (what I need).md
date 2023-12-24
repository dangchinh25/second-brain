# API-Only Applications
 - Generate new applications: `$ rails new blog --api` (for API only application)
	 - Configure application to start with a more litmited set of middleware than normal. Specifically, it will not include any middleware primarily useful for browser applications
	 - Make `ApplicationController` inherit from `ActionController::API` instead of `ActionController::Base`. As with middleware, this leaves out any Action Controller module that provide functionality primarily used by browser applications
	 - Configure the generators to skip generating views, helpers, and assets when generate a new resource

- In development environment, Rails does not require you to restart the server, changes you make in files will be automatically picked up by the server
- Run the application `$ bin/rails server`
- **NOTES**: We probably gonna use some other testing framework (e.g Rspec) over the builtin testing tool so should add `-T` to skip the creation of the test directory

## Custom Database
- Rails use the default database SQLite
- To use a custom database (MySQL, Postgres, etc), when we create a new application, we need to pass in the argument `-d <database>`
- E.g: `$ rails new blog --api -d mysql`
- After that we need to config the `config/database.yml` file 
	- Update the username and password for our database under `default` group
	- The name for database will be under `development` and `test` group (both groups extend the `default` group and only add in the database name field)
- Run `bin/rails db:create` to create the database
- Refer to [[Rails for API only (what I need)#Migration | Migration]] for further info

## Application File Structure
- app/: Contains the controller, models, views, helpers, mailers, channels, jobs, and assets for your application. 
- bin/: Contains the rails script that starts your app and can contain other scripts you use to set up, update, deploy, or run your application
- config/: Contains configuration for your applications's routes, database, and more
- config.ru: Rack configuration for Rack-based servers used to start the application
- db/: Contains your current database schema, as well as the database migration
- Gemfile + Gemfile.lock: These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem.
- lib/: Extended modules for your application
- log/: Application log files
- public/: Contains static files and compiled assets. When your app is running, this directory wil be exposed as-is
- Rakefile: this file locates and loads task that can be run from the command line. The task definition are defined throughout the components of Rails. Rather than changing the Rakefile, you should add your own tasks by adding files to the lib/tasks directory of your application
- README.md: This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on
- storage/: Active Storage files for Disk Service
- test/: Unit tests, fixtures, and other test appartus.
- tmp/: Temporary files (like cache and pid files)
- vendor/: A place for all third-party code. In a typical Rails application this includes vendored gems
- ruby-version: This file contains the default Ruby version

## Model
- A `model` is a Ruby class that is used to represent data. Additionally, models can interact with the application's database through a feature of Rails called `Active Record`
- To define a model
`$ bin/rails generate model Article title:string body:text`
- Model names are **singular**, because an instantiated model represents a single data record.
![](https://i.imgur.com/2xLzou1.png)
- Ref: [[Active Records]]

## Controllers
- Ref: [[Controllers]]

## Migration
- `Migration` are used to alter the structure of an application's database. In Rails applications, migrations are written in Ruby so that they can be database-agnostic
![](https://i.imgur.com/TIZiuiq.png)
- The call to `create_table` specifies how the `articles` table should be constructed. By default, `create_table` method adds an `id` columns as an auto-incrementing primary key.
- `t.timestamps` add two additional columns named `created_at` and `updated_at` that Rails will manage for us
- Run migration: `bin/rails db:migrate`
![](https://i.imgur.com/fNq5GVO.png)
- We can also interact with the rail console and thus create a new row in database by `bin/rails c` and it will load up `irb (interactive ruby)`

## Resourceful Routing
- Rails provides a route method names `resources` that maps all of the conventional routes for a collection of resources.
![](https://i.imgur.com/eaivoB5.png)
- We can inspect what routes are mapped by running
![](https://i.imgur.com/VH6xU3m.png)
- The `resources` method sets up URL and path helper methods that we can use to keep our code from depending on a specific route configuration. The value in the `Prefix` column above plus a suffix of `_url` or `_path` form the names of these helper.
	- E.g: `article_path` returns `/articles/#{article.id}` when given an article
- Ref: [[Routes]]

## Autoload
- During the initialization process of Rails app bundler loads `boot.rb` and `application.rb` (which underneath use the [[Gem#Load Path | `LOAD_PATH`]] to load required gem)
- In a normal Ruby Program, dependencies need to be loaded by hand (check [[Ruby#Library | Library]])
- This is not the case in Rails application, where application classes and modules are just available everywhere
> -  We refer to the list of applications directories whose contents are to be autoloaded and reloaded as *autoload path*.
> - By default the autoload paths of an application consist of all the subdirectories of `app` that exist when the application boots --except for assets, javascript, and views
> - Rails adds custom directories under `app` to the autoload path automatically. E.g: if your application has `app/presenters`, you don't need to configure anything in order to autoload presenters, it works out of the box
- Rails application only issue `require` calls to load stuff from their `lib` directory, the Ruby standard library, etc => anything that does not belong to their autoload path
- File names **have to match** the thing they define for autoload to work
- E.g: 
	- The file `app/helpers/users_helper.rb` should define `UsersHelper` 
	- This is because Rails camelize the filename to get the thing they define 
> "users_controller".camelize = "UsersController