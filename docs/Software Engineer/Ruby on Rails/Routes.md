# Routes
## Basic
### Connect URLs to Code
- When your Rails application receives an incoming request for
> GET /patients/17
- It asks the router to match it to a controller action. If the first matching route is
```rb
get '/patients/:id', to: 'patients#show'
```
- The request is dispatched to the `patients` controller's `show` action with `{id: '17'}` in `params`
- Rails uses snake_case for controller names here, if you have a multiple word controller like `MonsterTruckController`, you want to use `monster_truck#show` for example 

### Configuring the Rails Router
- The router for your application or engine live in the file `config/routes.rb` and typically looks like this
![](https://i.imgur.com/Wq7RkuJ.png)

### Routes Controller Mapping
- Rails will map the route to its corresponding controller according to this syntax 
> resource :<resource_name> => <resource_name>Controller
- E.g: `resources :brands => BrandsController, :basket => BasketController`
- **NOTES**: the mapping only work if the controllers are directly under `app/controllers` directory, for further customization, see [[Routes#Namespace | Namespace]]

## Resource Routing: the Rails default
- Resource routing allows you to quickly declare all of the common routes for a given resourceful controller. 
- A single call to `resource` can declare all the necessary routes
	- index
	- show
	- new
	- edit
	- create
	- update
	- destroy
- A resourceful route provides a mapping between HTTP verbs and URLs to controller actions.
![](https://i.imgur.com/ylsaTDn.png)
- **NOTES**: If. using `--api` when creating application, the route `index` and `new` will not be created (as we are dealing with [[Rails for API only (what I need) | API-only application]])
### Singular Resource
- For example, you would like `/profile` to always show the profile of the currently logged in user. In this case you can use a singular resource to map `/profile` to the `show` action
> get 'profile', to: 'users#show'
- Passing a `String` to `to:` will expect a `controller#action` format. When using a `Symbol`, the `to:` option should be replaced with `action:`. When using a `String` without a `#`, the `to:` option should be replaced with `controller:`
> get 'profile', action: :show, controller: 'users'

### Namespace
- You may wish to organize groups of controllers under a namespace.
- For example: you might group a number of administrative controllers under and `Admin::` namespace, and place these controllers under the `app/controllers/admin` directory. You can route to such a group by using a `namespace block`
![](https://i.imgur.com/q2IFVcA.png)
- This will create a number of routes for each of the `articles` and `comments` controller. For `Admin::ArticlesController`, Rails will create
![](https://i.imgur.com/ZS58Sqn.png)

### Nested Resources
- It's common to have resources that are logically children of other resources.
- E.g
![](https://i.imgur.com/TlL7uF4.png)
- Nested routes allow you to capture this relationship in your routing. In this case, you could include this in route declartion
![](https://i.imgur.com/jcHqDUs.png)

![](https://i.imgur.com/M8DlLvs.png)

### Adding More RESTful Actions
- We are not limited to the 7 routes that RESTful routing creates by default
- To add a member route, just add a `member` block into the resource block
![](https://i.imgur.com/9OAxKa2.png)
- This will recognize `/photos/1/preview` with GET, and route to the `preview` action of `PhotosController`, with the resource id value passed in `params[:id]`