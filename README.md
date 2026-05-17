# Learn Rust

cargo pkg manager
TOML (Tom’s Obvious, Minimal Language) is a cargo configuration format

cargo build
- creates an executable file in target/debug/hello_cargo (or target\debug\hello_cargo.exe on Windows) rather than in your current directory. Because the default build is a debug build

## Chapter 1
### RUN
./target/debug/hello_cargo     OR    cargo run
if the file not change so it didnt rebuild

cargo check: checks your code to make sure it compiles but doesn’t produce an executable

### Release
cargo build --release
compile it with optimizations

---

## Chapter 2
standard library docs: https://doc.rust-lang.org/std/prelude/index.html
rust variables is immutable by default
```rust
let apples = 5; // immutable
let mut bananas = 5; // mutable
```
The :: syntax in the ::new line indicates that new is an associated function of the String type

The & indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times.


### Crate
crate is a collection of Rust source code files. The project we’ve been building is a binary crate, which is an executable. The rand crate is a library crate, which contains code that is intended to be used in other programs and can’t be executed on its own.

#### Cargo semver
Cargo understands Semantic Versioning 

When we include an external dependency, Cargo fetches the latest versions of everything that dependency needs from the registry, which is a copy of data from Crates.io. Crates.io is where people in the Rust ecosystem post their open source Rust projects for others to use.

#### Cargo lock
Cargo will use only the versions of the dependencies you specified until you indicate otherwise
Cargo.lock file the first time you run cargo build
lock dependencies

#### Update crate
When you do want to update a crate, Cargo provides the command update, which will ignore the Cargo.lock file and figure out all the latest versions that fit your specifications in Cargo.toml.

---

## Chapter 3
```rust
// Rust’s naming convention for constants is to use all uppercase with underscores between words. 
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;

// first variable is shadowed by the second, which means that the second variable is what the compiler will see when you use the name of the variable.
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}

let spaces = "   ";
let spaces = spaces.len();


```

Rust is statically type language

Shadowing thus spares us from having to come up with different names, such as spaces_str and spaces_num; instead, we can reuse the simpler spaces name

A scalar type represents a single value. Rust has four primary scalar types: 
- integers: 8-128 bit signed/unsigned AND isize and usize types depend on the architecture of the computer your program
- floating-point 
- Booleans
- characters

### Integer Literals
Decimal	98_222
Hex	0xff
Octal	0o77
Binary	0b1111_0000
Byte (u8 only)	b'A'

### Number Overflow
To explicitly handle the possibility of overflow, you can use these families of methods provided by the standard library for primitive numeric types:

Wrap in all modes with the wrapping_* methods, such as wrapping_add.
Return the None value if there is overflow with the checked_* methods.
Return the value and a Boolean indicating whether there was overflow with the overflowing_* methods.
Saturate at the value’s minimum or maximum values with the saturating_* methods.

### Float
```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

### Bool
```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

### Character
```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```

### Compund types
Group multiple values into 1 type

#### Tuple
```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}

fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```

#### Array
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}

let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];

let a: [i32; 5] = [1, 2, 3, 4, 5];

let a = [3; 5]; // same as let a = [3, 3, 3, 3, 3];

// array element access
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

// Index out of bound
thread 'main' panicked at src/main.rs:19:19:
index out of bounds: the len is 5 but the index is 10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

### Functions
```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

### Parameters
```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}

// Multiple parameter
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

### Expression & Statement
```rust
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
error: expected expression, found `let` statement
 --> src/main.rs:2:14
  |
2 |     let x = (let y = 6);
  |              ^^^
  |
  = note: only supported directly in conditions of `if` and `while` expressions

warning: unnecessary parentheses around assigned value
 --> src/main.rs:2:13
  |
2 |     let x = (let y = 6);
  |             ^         ^
  |
  = note: `#[warn(unused_parens)]` on by default
help: remove these parentheses
  |
2 -     let x = (let y = 6);
2 +     let x = let y = 6;
  |

warning: `functions` (bin "functions") generated 1 warning
error: could not compile `functions` (bin "functions") due to 1 previous error; 1 warning emitted
```
---

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

### Functions with return values
We don’t name return values, but we must declare their type after an arrow (->). In Rust, the return value of the function is synonymous with the value of the final expression in the block of the body of a function.
```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```

Use semicolon when you're doing something (statement)
Don't use semicolon when you're returning something (expression) at the end of a function

### Control flow
```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

It’s also worth noting that the condition in this code must be a bool!!
```rust
$ cargo run
   Compiling branches v0.1.0 (file:///projects/branches)
error[E0308]: mismatched types
 --> src/main.rs:4:8
  |
4 |     if number {
  |        ^^^^^^ expected `bool`, found integer

For more information about this error, try `rustc --explain E0308`.
error: could not compile `branches` (bin "branches") due to 1 previous error

```

### Else if
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

### If in a let statement
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

---

```rust
fn main() {
    let condition = true;

    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {number}");
}

$ cargo run
   Compiling branches v0.1.0 (file:///projects/branches)
error[E0308]: `if` and `else` have incompatible types
 --> src/main.rs:4:44
  |
4 |     let number = if condition { 5 } else { "six" };
  |                                 -          ^^^^^ expected integer, found `&str`
  |                                 |
  |                                 expected because of this

For more information about this error, try `rustc --explain E0308`.
error: could not compile `branches` (bin "branches") due to 1 previous error

```
if and else arms have value types that are incompatible,

### Loop
```rust
fn main() {
    loop {
        println!("again!");
    }
}

// return value from loop
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

### Loop in loop??
```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

### While loop
```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

### Loop collection with for
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index += 1;
    }
}

fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```

## Chapter 4

Ownership is a set of rules that govern how a Rust program manages memory. All programs have to manage the way they use a computer’s memory while running.

Memory is managed through a system of ownership with a set of rules that the compiler checks. If any of the rules are violated, the program won’t compile. None of the features of ownership will slow down your program while it’s running.

### Ownership Rules
Each value in Rust has an owner.
There can only be one owner at a time.
When the owner goes out of scope, the value will be dropped.

```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{s1}, world!");

$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0382]: borrow of moved value: `s1`
 --> src/main.rs:5:16
  |
2 |     let s1 = String::from("hello");
  |         -- move occurs because `s1` has type `String`, which does not implement the `Copy` trait
3 |     let s2 = s1;
  |              -- value moved here
4 |
5 |     println!("{s1}, world!");
  |                ^^ value borrowed here after move
  |
  = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
3 |     let s2 = s1.clone();
  |                ++++++++

For more information about this error, try `rustc --explain E0382`.
error: could not compile `ownership` (bin "ownership") due to 1 previous error
```

Therefore, any automatic copying can be assumed to be inexpensive in terms of runtime performance.


### Clone
If we do want to deeply copy the heap data of the String, not just the stack data, we can use a common method called clone
```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {s1}, s2 = {s2}");
```

### Stack-Only Data: Copy
```rust
let x = 5;
let y = x;

println!("x = {x}, y = {y}");
```
The reason is that types such as integers that have a known size at compile time are stored entirely on the stack, so copies of the actual values are quick to make


### Ownership and functions
```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // Because i32 implements the Copy trait,
                                    // x does NOT move into the function,
                                    // so it's okay to use x afterward.

} // Here, x goes out of scope, then s. However, because s's value was moved,
  // nothing special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope. Nothing special happens. 
```

### References and Borrowing
we take &String rather than String. These ampersands represent references, and they allow you to refer to some value without taking ownership of it.

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

The opposite of referencing by using & is dereferencing, which is accomplished with the dereference operator, *

```rust
let s1 = String::from("hello");
let len = calculate_length(&s1);
```

So, what happens if we try to modify something we’re borrowing? Try the code in Listing 4-6. Spoiler alert: It doesn’t work!
```rust
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}

$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0596]: cannot borrow `*some_string` as mutable, as it is behind a `&` reference
 --> src/main.rs:8:5
  |
8 |     some_string.push_str(", world");
  |     ^^^^^^^^^^^ `some_string` is a `&` reference, so the data it refers to cannot be borrowed as mutable
  |
help: consider changing this to be a mutable reference
  |
7 | fn change(some_string: &mut String) {
  |                         +++

For more information about this error, try `rustc --explain E0596`.
error: could not compile `ownership` (bin "ownership") due to 1 previous error
```

Mutable references have one big restriction: If you have a mutable reference to a value, you can have no other references to that value. This code that attempts to create two mutable references to s will fail:
```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;

println!("{r1}, {r2}");

$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0499]: cannot borrow `s` as mutable more than once at a time
 --> src/main.rs:5:14
  |
4 |     let r1 = &mut s;
  |              ------ first mutable borrow occurs here
5 |     let r2 = &mut s;
  |              ^^^^^^ second mutable borrow occurs here
6 |
7 |     println!("{r1}, {r2}");
  |                -- first borrow later used here

For more information about this error, try `rustc --explain E0499`.
error: could not compile `ownership` (bin "ownership") due to 1 previous error

The benefit of having this restriction is that Rust can prevent data races at compile time. A data race is similar to a race condition and happens when these three behaviors occur:

Two or more pointers access the same data at the same time.
At least one of the pointers is being used to write to the data.
There’s no mechanism being used to synchronize access to the data.
```

### Allowing multiple mutable references
```rust
let mut s = String::from("hello");

{
    let r1 = &mut s;
} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
let r3 = &mut s; // BIG PROBLEM

println!("{r1}, {r2}, and {r3}");

$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
 --> src/main.rs:6:14
  |
4 |     let r1 = &s; // no problem
  |              -- immutable borrow occurs here
5 |     let r2 = &s; // no problem
6 |     let r3 = &mut s; // BIG PROBLEM
  |              ^^^^^^ mutable borrow occurs here
7 |
8 |     println!("{r1}, {r2}, and {r3}");
  |                -- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `ownership` (bin "ownership") due to 1 previous error
```

Whew! We also cannot have a mutable reference while we have an immutable one to the same value.

Users of an immutable reference don’t expect the value to suddenly change out from under them! However, multiple immutable references are allowed because no one who is just reading the data has the ability to affect anyone else’s reading of the data.

Note that a reference’s scope starts from where it is introduced and continues through the last time that reference is used. 
```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{r1} and {r2}");
    // Variables r1 and r2 will not be used after this point.

    let r3 = &mut s; // no problem
    println!("{r3}");
```

### Dangling References
```rust
fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope and is dropped, so its memory goes away.
  // Danger!
```

the caller that will be have the ownership

### Slices
```rust
let mut s = String::from("hello world");
// Option 1: clear() - Empties the string but KEEPS the capacity
s.clear();  // String becomes "", but memory is still allocated
println!("{}", s);  // "" (empty)
println!("Capacity: {}", s.capacity());  // Still has allocated memory!
```

---

```rust
fn first_word(s: &String) -> usize {
    // Returns the INDEX where the first word ends
    // Example: "hello world" → returns 5
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }

    s.len()
}


fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s); // word will get the value 5

    s.clear(); // this empties the String, making it equal to ""

    // word still has the value 5 here, but s no longer has any content that we
    // could meaningfully use with the value 5, so word is now totally invalid!
}
```

#### String Slices
```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```

Rather than a reference to the entire String, hello is a reference to a portion of the String,
specified in the extra [0..5] bit.

```rust
let s = String::from("hello");
// s is on stack: [ptr: 0x1000, len: 5, cap: 5]
// heap at 0x1000: [h][e][l][l][o]

let r1: &String = &s;
// r1 points to s's STRUCT on STACK
// r1: [ptr_to_s_on_stack]

let r2: &str = &s[..];
// r2 points DIRECTLY to heap data
// r2: [ptr: 0x1000, len: 5]
```

&String = Reference to a String (pointer to the String struct on stack)
&str = Reference to a string slice (pointer directly to heap data + length)

## Chapter 5
```rust
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}
```
Note that the entire instance must be mutable; Rust doesn’t allow us to mark only certain fields as mutable. 

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```

#### Field Init Shorthand
```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```
Because the email field and the email parameter have the same name, we only need to write email rather than email: email


#### Creating instances with struct update syntax
```rust
fn main() {

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
}

fn main() {

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

#### Tuple structs 
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

#### Unit like structs
```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

#### Lifetime
```rust
struct User {
    active: bool,
    username: &str,
    email: &str,
    sign_in_count: u64,
}

fn main() {
    let user1 = User {
        active: true,
        username: "someusername123",
        email: "someone@example.com",
        sign_in_count: 1,
    };
}

$ cargo run
   Compiling structs v0.1.0 (file:///projects/structs)
error[E0106]: missing lifetime specifier
 --> src/main.rs:3:15
  |
3 |     username: &str,
  |               ^ expected named lifetime parameter
  |
help: consider introducing a named lifetime parameter
  |
1 ~ struct User<'a> {
2 |     active: bool,
3 ~     username: &'a str,
  |

error[E0106]: missing lifetime specifier
 --> src/main.rs:4:12
  |
4 |     email: &str,
  |            ^ expected named lifetime parameter
  |
help: consider introducing a named lifetime parameter
  |
1 ~ struct User<'a> {
2 |     active: bool,
3 |     username: &str,
4 ~     email: &'a str,
  |

For more information about this error, try `rustc --explain E0106`.
error: could not compile `structs` (bin "structs") due to 2 previous errors

```

Rust needs lifetimes to prove, at compile time, that every reference always points to valid data — without needing a garbage collector.

For simple functions, Rust infers lifetimes automatically:
```rust
fn first_word(s: &str) -> &str {  // lifetimes inferred, no annotation needed
    &s[0..1]
}
```

But for structs holding references, the compiler hits a wall:
```rust
struct User {
    username: &str,  // Who owns this string? How long does it live?
    email: &str,     // Same question.
}
```

#### Three ways to share data
1. Clone it (wasteful)
```rust
let name = String::from("Alice");

let user1 = User { username: name.clone(), email: ... };
let user2 = User { username: name.clone(), email: ... };

// Two copies of "Alice" exist now. Memory waste.
```

2. Move it (can't reuse)
```rust
let name = String::from("Alice");

let user1 = User { username: name, email: ... };
// name is gone. Can't use it for anything else.
```

3. Borrow it (efficient, but needs lifetimes)
```rust
let name = String::from("Alice");

let user1 = User { username: &name, email: ... };
let user2 = User { username: &name, email: ... };
// Both point to the SAME 'name'. One copy. Efficient.

println!("{}", name);  // We can still use 'name'!
```

Why NOT always use references?
Because references create chains of dependency that can strangle your program's flexibility.

#### Real scenarios where references shine
1. Large data you don't want to copy
```rust
let huge_text = load_entire_book();  // 10MB string

// Without borrowing, each SearchResult would clone parts of this
// With borrowing, they just point into it:
struct SearchResult<'a> {
    snippet: &'a str,  // Just a pointer, zero copy
    page_number: usize,
}

let results: Vec<SearchResult> = search(&huge_text, "whale");
// All results point into huge_text. No cloning 10MB strings.
```

2. Multiple "views" into the same data
```rust
let data = vec![1, 2, 3, 4, 5, 6, 7, 8];

let first_half = &data[0..4];   // Just a view
let second_half = &data[4..8];  // Another view
let all_of_it = &data[..];      // Yet another view

// Three references, ONE allocation. Zero copies.
```

3. Temporary access without taking ownership
```rust
fn print_user(user: &User) {  // Borrow, don't take ownership
    println!("{}", user.username);
}

let user = User { username: String::from("Alice"), ... };

print_user(&user);  // Lends the user temporarily
print_user(&user);  // Can do it again
// user still owns the data, we can keep using it
```

#### Adding Functionality with Derived Traits
When you create a custom struct in Rust, you can't just print it using println!("{}", my_struct) because Rust doesn't know how to display it. (Struct, Enums)

```rust
println!("rect1 is {rect1}");  // Error! Rectangle doesn't implement Display
println!("rect1 is {rect1:?}");  // Still error! Rectangle doesn't implement Debug

#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

// Now this works!
println!("rect1 is {rect1:?}");
// Output: rect1 is Rectangle { width: 30, height: 50 }
```

### Why Use Methods Instead of Functions?
The main benefit is organization and discoverability:
```rust
// All Rectangle-related functionality in one place
impl Rectangle {
    fn area(&self) -> u32 { }
    fn perimeter(&self) -> u32 { }
    fn can_hold(&self, other: &Rectangle) -> bool { }
    fn square(size: u32) -> Rectangle { }  // Associated function (no self!)
}

// Users can easily find what Rectangle can do
// No need to search through entire codebase
```

### Self parameter & variations
```rust
impl Rectangle {
    // &self = "I borrow myself immutably (can read, can't change)"
    fn area(&self) -> u32 {
        self.width * self.height  // self refers to the instance
    }
    
    // &mut self = "I borrow myself mutably (can change myself)"
    fn double_size(&mut self) {
        self.width *= 2;
        self.height *= 2;
    }
    
    // self = "I take ownership of myself" (rare!)
    fn destroy(self) {
        // After this, the instance is gone
    }
}

// VARIATION
struct Counter {
    count: u32,
}

impl Counter {
    // &self: Read-only access (most common)
    fn get_count(&self) -> u32 {
        self.count
    }
    
    // &mut self: Can modify the instance
    fn increment(&mut self) {
        self.count += 1;
    }
    
    // self: Takes ownership, destroys the original
    fn consume(self) -> u32 {
        self.count  // After this, 'self' is gone
    }
}

fn main() {
    let mut counter = Counter { count: 0 };
    
    println!("{}", counter.get_count());  // Works
    counter.increment();                   // Works (needs mut)
    println!("{}", counter.get_count());  // Still works!
    
    let value = counter.consume();         // Takes ownership
    // counter.get_count();  // ERROR! counter was consumed
}
```