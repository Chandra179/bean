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