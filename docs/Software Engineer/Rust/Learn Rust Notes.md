
- Rust file extension is `*.rc`
- Similar to C/C++, we have to run `rustrc file.rs` to build and generate an executable file `file` and run it by `./file`
- Most people use `cargo`, it's similar to `npm` in Javascript
	- Manage dependencies
	- Can build the whole project with using `cargo build`
	- Can also execute the code immediately using `cargo run <path>`
  - Rust use *snake_case*

- Variables
	- Immutable by default: `let x = 5;`
	- To make it be mutable: `let mut x = 5;`
	- Constant: `const x: i32 = 10;` => Need to specify a type, can't use `mut`
	- Can redeclare a variable with a different name to *shadow*, basically creates a copy of the original variable and any change to the shadow variable does not affect the main one.
		- In effect, the second variable overshadows the first, taking any uses of the variable name to itself until either it itself is shadowed or the scope ends.
```rust
let x = 5;
let x = 6;

println!("x is {}", x) // Print: x is 6
```

- Functions
```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

- Rust doesn’t care where you define your functions, only that they’re defined somewhere in a scope that can be seen by the caller.
- **Statements** are instructions that perform some action and do not return a value.
- **Expressions** evaluate to a resultant value
```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```
- Anything without the semi-colon is an expression, that will return a value. If we add a semi-colon, it becomes a statement.
- Note that using expression does not always mean return. If we want to return early in a function we should explicitly use the `return` keyword

- If/else
	- Unlike Javascript, Python where variable can be automatically converted to Falsy, Truthy value, we have to use a Boolean
```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```
- We can also assign `if` to let as `if` is an expression
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

- Data Types
	- Need to distinguish between character and string literals: single quote vs double quote
```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```

- Support tuple types
```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);

	 let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
	
	let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

- Slice Type
```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```
- Rather than a reference to the entire `String`, `hello` is a reference to a portion of the `String`, specified in the extra `[0..5]` bit. 
- We create slices using a range within brackets by specifying `[starting_index..ending_index]`, where `starting_index` is the first position in the slice and `ending_index` is **one more than the last position** in the slice. 
- Internally, the slice data structure stores the starting position and the length of the slice, which corresponds to `ending_index` minus `starting_index`. So, in the case of `let world = &s[6..11];`, `world` would be a slice that contains a pointer to the byte at index 6 of `s` with a length value of `5`.

## Memory and Allocation
- In the case of a string literal, we know the contents at compile time, so the text is hardcoded directly into the final executable. This is why string literals are fast and efficient => It is using [[Memory#Stack | Stack]] memory. 
- With the `String` type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the [[Memory#Heap | Heap]], unknown at compile time.
	- The memory must be requested from the memory allocator at runtime.
	- We need a way of returning this memory to the allocator when we're done with out `String`
- The memory requested part is done by use by calling `String::from`, this is pretty much universal in programming language.
- The returning heap memory is different.
	- In languages with **garbage collector - GC**, the GC keeps track of and cleans up memory that isn't being used anymore, and we don't need to think about it. 
	- In most languages without a GC, it's our responsibility to identify when memory is no longer being used and to call code to explicitly free it => Difficult

- RUST takes a different path, the memory is automatically returned once the variable that owns it goes out of scope. 
```rust
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid
```
- When a variables goes out of scope, Rust calls a special function for us at the closing curly bracket.
### Variables and Data interacting with Move
- Multiple variables can interact with the same data in different ways in Rust. 
```rust
    let x = 5;
    let y = x;
```
- We can probably guess what this is doing: “bind the value `5` to `x`; then make a copy of the value in `x` and bind it to `y`.” We now have two variables, `x` and `y`, and both equal `5`. This is indeed what is happening, because integers are simple values with a known, fixed size, and these two `5` values are pushed onto the **stack**.

```rust
    let s1 = String::from("hello");
    let s2 = s1;
```
- This is different from what is happening in the above example.
![](https://i.imgur.com/ZFUwubK.png)
- A string is made up of three part, show on the *left*: a pointer to the memory that holds the contents of the string, a length, and a capacity. This group of data is store on the **stack**. On the *right* is the memory on the **heap** that holds the contents.
- When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack. We do not copy the data on the heap that the pointer refers to. In other words, the data representation in memory looks like this.
![](https://i.imgur.com/0wYQwHY.png)
- This creates a problem, if both `s1` and `s2` go out of scope, Rust will try to free the memory (call the `drop` function) twice => Which will lead to error.
- To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as no longer valid. Therefore, Rust doesn’t need to free anything when `s1` goes out of scope.
- We can also understand this as `s2` is a *shallow copy* of `s1`, but Rust also invalidates the first variables, so it is called *moved* => `s1` was move into `s2`
- This is what actually happens:
![](https://i.imgur.com/1Sej5nS.png)

### Variables and Data interacting with Clone
- If we do want to deeply copy the **heap** data of the `String`, we can use `clone`
```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```
- There is a contradiction
```rust
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
```
- We don't have to call `clone`  but `x` is still valid and wasn't moved into `y`
- The reason is that types such as integer that have a know size at compile time are stored entirely on the **stack**, so copies of the actual values are quick to make => calling `clone` wouldn't do anything different.
- Rust has a special annotation called the `Copy` *Trait* that we can place on types that are stored on the stack. If a type implements the `Copy` trait, variables that use it do not `move`, but rather are trivially copied, making them still valid after assignment to another variable