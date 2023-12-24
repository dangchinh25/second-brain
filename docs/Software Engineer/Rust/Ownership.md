# Function
- The mechanics of passing a value to a function are similar to those when assigning a value to a variable. Passing a variable to a function will move or copy, just as assignment does. 
```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```
- If we tried to use `s` after the call to `takes_ownership`, Rust would throw a compile-time error.
## Return Value and Scope
- Returning values can also transfer ownership.
```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

- What if we want to let a function use a value but not take ownership => Anything we pass in also needs to be passed back if we want to use it again, in addition to any resulting data => *Returns multiple values as a tuple*
```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

- These are all too much and a lot of work for a concept that should be common => Use [[Ownership#References and Borrowing | References]] instead
# References and Borrowing
- The current issue with returning the tuple code is that we have to return the passed in value to the calling function so we can still use the original value as the function already *take ownership of the variable/the value was moved into the function*.
- Instead, we can provide a *References* to the passed in value. A references is like a pointer in that it's an address we can follow to access the data stored at that address, that data is owned by some other variable. Unlike pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference.
```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // Here, s goes out of scope. But because it does not have ownership of what // it refers to, it is not dropped.
```
- The ampersands represent *References*, and they allow you to refer to some value without taking ownership of it.
![](https://i.imgur.com/jLZpBX6.png)
- The scope in which `s` is valid is the same as any function parameter's scope, but the value pointed to by the reference is not dropped when `s` stops being used as `s` does not have ownership.
- The action of creating a *References* is **borrowing**. As in real life, if a person owns something, you can borrow it from them and you have to give it back when you are done, you don't own it.
- Similar to variables, *References* are also immutable by default.
## Mutable References
- If we actively want to modify a value using a function, we can use *Mutable References*
```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
- This make very clear that the function will mutate the value it borrows.
- If you have mutable reference to a value, you can have no other references to that value until that mutable references has been used.
```rust
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2); 

	// THIS WILL NOT COMPILE
```
- The first mutable borrow is in `r1` and must last until it’s used in the `println!`, but between the creation of that mutable reference and its usage, we tried to create another mutable reference in `r2` that borrows the same data as `r1`.
- This prevents **Data Races** => Rust prevents this problem in compile time.
	- Two or more pointers access the same data at the same time.
	- At least one of the pointers is being used to write to the data.
	- There’s no mechanism being used to synchronize access to the data
- We can allow multiple mutable references, just not *simultaneous one*
```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;
```

- Rust enforces a similar rule for combining mutable and immutable references.
```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    let r3 = &mut s; // BIG PROBLEM

    println!("{}, {}, and {}", r1, r2, r3);
	// THIS WILL NOT COMPILE
```
- Users of an immutable reference don’t expect the value to suddenly change out from under them! 
- However, multiple immutable references are allowed because no one who is just reading the data has the ability to affect anyone else’s reading of the data.
- This works as the last usage of the immutable references occurs before the mutable reference is introduced
```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```

## Dangling References
- In languages with pointers, it's easy to create a **Dangling pointer** - a pointer that references a location in memory that may have been given to someone else.
- Rust guarantees that references will never be dangling references: 
> If you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does
```rust
fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger!
```
- Because `s` is created inside `dangle`, when the code of `dangle` is finished, `s` will be deallocated. But we tried to return a reference to it. That means this reference would be pointing to an invalid `String`.