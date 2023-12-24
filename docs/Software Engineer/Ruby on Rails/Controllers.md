# Controllers
## Naming Conventions
- The naming convention of controllers in Rails favors pluralization of the last word in the controller name
	- `ClientsController` is preferable to `ClientController`
	- `SiteAdminsController` is preferable to `SiteAdminController` 
- Following this convention will allow you to use the default route generators (e.g `resources`) without needing to qualify each `:path` or `:controller`        

## Methods and Actions
- A controller is a Ruby class which inherits from `ApplicationController` and has methods just like any other class
- When the application receives a request, the [[Routes#Routes | routing]] will determine which controller and action to run, the Rails creates an instance of that controller and runs the method with the same name as the action
![](https://i.imgur.com/F5pivAW.png)
- As an example, if a user goes to `/clients/new` in the application to add a new client, Rails will create an instance of `ClientController` and call its `new` method

## Parameters
- There are 2 kinds of parameters possible in a web application: query string and POST data body
- Rails does not make any distinction between these 2 form, and both are available in the `params` hash in your controller
![](https://i.imgur.com/c0MUwqQ.png)
### JSON parameters
- We often accept parameters under JSON format and Rails will automatically load the parameters into the `params` hash
- E.g
![](https://i.imgur.com/6kuU8J7.png)

### Strong Parameters
- We want to control the parameters passed to the controllers and only let the allowed properties goes through and also verify if the required properties exists
![](https://i.imgur.com/CGyWXS7.png)

- `require`:  the `params` that get passed must cointain a key called "post", if not then it fails and the user gets an error
- `permit`: the `params` hash may have whatever keys are in it, so in the `create`, it may have the `:title` and `:description` keys. If it doesn't have one of those keys it just won't accept any other keys

## Filters
- Filters are methods that are run "before", "after", or "around" a controller action
- Filters are inhertied, so if you set a filter on `ApplicationController`, it will be run on every controller in your application

### Before filter
- Registered via `before_action`
![](https://i.imgur.com/vsYexqu.png)

### After filter
- Registered via `after_action`. 
- As the action has already been run they have access to the response data that's about to be sent to the client
- `after` filters are executed only after a successful action, but not when an exception is raised 

## Rescue
- We can use `rescue_from`, which handles exceptions of a certain type (or multiple types) in an entire controller and its subclasses
- When an exception occurs which is caught by a `rescue_from` directive, the exception object is passed to the handler. 
![](https://i.imgur.com/PsvVvY0.png)
