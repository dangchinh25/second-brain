# Refresher
```go
type MyStruct struct {
    Val int
}

func myfunc() MyStruct {
    return MyStruct{Val: 1}
}

func myfunc() *MyStruct {
    return &MyStruct{}
}

func myfunc(s *MyStruct) {
    s.Val = 1
}
```
- The first returns a copy of the struct, 
- The second a pointer to the struct value created within the function
- The third expects an existing struct to be passed in and overrides the value

## Struct & Embedding
- **Short summary**: An assignment to a variable of interface type is valid the value being assigned implemented all of the method of the interface is is assigned to. It implements if its *method set* is a superset of the interface. The method set of pointer types is a **super set of both pointer and non-pointer receiver**. The method set non-pointer types only includes methods with non-pointer receiver.
- Basically this means that if we want to use an interface that requires many method, we need to pair it with a pointer point to a struct that implements all that method 
	- The reason for this is method is not implemented IN the struct, but we use this syntax `func (*myStruct) myFunc() {}`, which means that we implement a method and add it to the **method set** of `*myStruct`, while other non-function non-pointer value is added to the **method set** of `myStruct`, so if we pair an interface with a non-pointer value, it will not see any of the implemented methods
# TLDR
- Similar idea to pointer in C/C++
- Obviously use pointer for method that modify the thing they're called on
- **Other than that it's pretty much preference of style & convention**
	- It's arguable to talk about performance as it does not improve that much to be matter
	- Sometime it makes sense to return a pointer
		- E.g: The function return a specific instance of something, doesn't make a lot of sense to generate duplicated/copied structures representing the same resources => returned pointer acts as the handler to the resource itself
		- E.g: `func NewServer(handler http.Handler) *Server` -- instantiate a web server
- "In general, share struct type values with a pointer unless the struct type has been implemented to behave like a primitive data value"
- Think of every struct as having a nature. If the nature of the struct is something that should not be changed, like a time, a color or a coordinate, then implement the struct as a primitive data value. If the nature of the struct is something that can be changed, even if it never is in your program, it is not a primitive data value and should be implemented to be shared with a pointer. Don't create structs that have a duality of nature
- **Summary**: If it is not for obvious reason and the performance is arguable (i.e small struct) don't use pointer