# Dependency Injection
## Overview
- Dependency Injection is the idea that your components should receive their dependencies when being created. This runs counter to the associated anti-pattern of components builiding their own dependencies during initialization. 
```go
type Server struct {
  config *Config
}

func New() *Server {
  return &Server{
    config: buildMyConfigSomehow(),
  }
}
```
- This may seems convenient at first as out caller doesn't have to be aware that our `Server` even needs access to `Config`
- But if we want to change the way our `Config` is built, we'll have to change all the places that call the building code. Suppose our build config function now needs an argument so every call site would need access to that argument and would need to pass it into the building function
- It is also really tricky to mock the behavior of our `Config`
```go
type Server struct {
  config *Config
}

func New(config *Config) *Server {
  return &Server{
    config: config,
  }
}
```
- Now the creation of our `Server` is decoupled from the creation of the `Config`. We can use whatever logic we want to create the `Config` and then pass the resulting data to our `New` function
- The down side is that it's a pain to have to manually create `Config` before we can create the `Server`. We've created a dependency graph here - `Server` depends on `Config`. 
- In a real application these depency graph can become very large 
- We can use a Dependency Injection Framework

## Dependency Injection Framework
- A DI Framework generally provides two pieces of functionality
	- A mechanism for "providing" new components. In a nutshell, this tells the DI framework what other components you need to build yourself (your dependencies) and how to build yourself once you have those component
	- A mechanism for "retrieving" built components
### Example
- Check: https://blog.drewolson.org/dependency-injection-in-go