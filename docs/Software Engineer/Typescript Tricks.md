
- As const: https://www.youtube.com/watch?v=6M9aZzm-kEc
- Use types over interface: https://www.youtube.com/watch?v=zM9UPcIyyhQ
- How to name new type: https://www.youtube.com/watch?v=qA65QjWCl60&t=10s
	- Always singular
	- Always use different casing for type name and variable of that type
	- When generic types has multiple type parameters, give all the parameters a descriptive name and prefix with a T to indicate it's a generic type
	![](https://i.imgur.com/sWV5Zgi.png)

- NoIner: https://www.youtube.com/watch?v=mkuChhuHO50
# Generics
- **Generics on type level**
![](https://i.imgur.com/KjJuWFv.png)
- **Passing types to your own functions**
	- We can create generic functions, which is just regular function which receive some additional type parameters
![](https://i.imgur.com/wWRu0Wz.png)
- **Inferring the types**
	- When we use a generic functions or generic class, Typescript can look at the runtime arguments that we pass in to infer the types, so we don't have to always manually pass the types
![](https://i.imgur.com/N2cvA11.png)

- **Constraint on type arguments**
	- Using the syntax `T extends ...` we can put a constraint on the type arguments that requires all the passed argument need to follow some kind of constraint (E.g: `T extends String` means that we can only pass in type arguments of type String or related, normally we want it to be some kind of complex object or functions)
	- The example below shows that we can enforce the type argument to be only of type functions
![](https://i.imgur.com/7cvQKJg.png)
	- Below is another example of case where we need to add constraint to the type argument
		- `Object.keys` only works with type of object, so we have to put that constraint on the type argument
> Generally, we would want to add constraint everywhere that we use generic functions
![](https://i.imgur.com/IENkSwH.png)

- **Multiple generics**
	- The example below is a great example of how we can use multiple generic type arguments
		- If we just setup like below, the key/result will be union of all the key/value inside the object, not by itself
	![](https://i.imgur.com/Xq3gdIs.png)
	- We can use multiple generic type argument to solve this
		- There is some automatic inference on the type level here
	![](https://i.imgur.com/s2LW9tN.png)

- **Default type arguments**
	![](https://i.imgur.com/etBjFBj.png)

# Misc
## Empty object type
![](https://i.imgur.com/rNByoCj.png)

# Conditional Types

# Mapped Types

When you use `C extends CollectionName` as a generic type parameter, it means that `C` can be any type that is a subtype of or the same as `CollectionName`. This allows for greater flexibility because you can use the function with a specific subtype of `CollectionName`. Here's a brief comparison:

1. **Using `C extends CollectionName` (with generic):**
   ```typescript
   export function create<C extends CollectionName>(c: C, data: CreateParams[C]) {
     // ...
   }
   ```

   In this case, when you call the function, you can provide a specific subtype for `C`:
   ```typescript
   create('specificCollection', specificData); // Here, C is inferred as 'specificCollection'
   ```

   This allows for a more specific and flexible use of the function with various subtypes of `CollectionName`.

2. **Using `c: CollectionName` (without generic):**
   ```typescript
   export function create(c: CollectionName, data: CreateParams[CollectionName]) {
     // ...
   }
   ```

   Here, the function only accepts the exact type `CollectionName` for the `c` parameter:
   ```typescript
   create('specificCollection', specificData); // Error, expects 'specificCollection' to be of type 'CollectionName'
   ```

   This approach is less flexible because it only works with the exact type specified (`CollectionName`), and you lose the ability to capture specific subtypes.

In summary, using `C extends CollectionName` with a generic allows for greater flexibility by letting the function work with specific subtypes of `CollectionName`. This can be particularly useful in situations where you want to ensure type safety while accommodating a range of related types.