# Notes for Rust Book

## Basics
- main.rs main function is the entry point to all programs
- Functions use curly brackets
- example_call calls a function, example_call! calls a rust macro. e.x. use println!
- f-string like functionality is this. The empty `{}` is an expression and will show results like formmating does in python.
    ```rust
    let x = 5;
    let y = 10;

    println!("x = {x} and y + 2 = {}", y + 2);

    ```
- A statement ends with `;`. An expression does not.
- Reference a variable with `&` before the variable name

## Variables
    ```rust
    //immutable
    let x = 5;

    //mutable
    let mut x = 5;
    ```
- Inline if statement
    ```rust
    let number = if x = 5  { "equals" } else { "not equals" };
    ```
- String literal vs String: A string literal is a known value is hardcoded into program. It is immutable A String is a variable type like other languages. It can be mutable. String literals are saved in the stack and are faster.
    ```rust
    let string_literal = "hello"
    let mut string = String::from("hello")

    string.push_str(", world!"); // push_str() appends a literal to a String
    ```

## Constants
Constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.

The last difference is that constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

Constants are valid for the entire time a program runs, within the scope in which they were declared. 

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

## Shadowing
Shadowing is using the same variable name multiple times.
```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```
```
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31s
     Running `target/debug/variables`
The value of x in the inner scope is: 12
The value of x is: 6
```

## Cargo
Cargo is overkill for basic projects as `rustc` will do. But it is super convinient for more complex projects.

### Basic Cargo Commands
- ```cargo new <project_name>``` makes a new project. It automatically makes it a git repository unless it already is. Output is saved to `./target/debug/`
- ```cargo build``` builds the project
- ```cargo run``` builds the project and runs it
- ```cargo check``` reviews code to see if it will build
- ```cargo build --release``` builds the project for release. It includes additional optimizations. Output is saved to `./target/release/`
- ```cargo update``` ignores the cargo.lock file and updates the versions with the latest.
- ```cargo doc --open``` builds documentation provided by all your dependencies locally and opens it in your browser.

## Numeric operations
```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // Results in -1

    // remainder
    let remainder = 43 % 5;
}
```

# Tuple Type
```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    // Access values by index
    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

## Array Type
Here, i32 is the type of each element. After the semicolon, the number 5 indicates the array contains five elements.
```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

You can also initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets, as shown here:
```rust
let a = [3; 5];
``````
The array named a will contain 5 elements that will all be set to the value 3 initially. This is the same as writing let a = [3, 3, 3, 3, 3]; but in a more concise way.

Get array values this way
```rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!("The value of the element at index {index} is: {element}");
}
```

## Looping
```rust
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

Loop lables when nested loops
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

Loop over collections
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```

Loop in reverse
```rust
fn main() {
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```

## Variable Slicing

It is similar to Python

```rust
let s = String::from("hello world");

// v1
let hello = &s[0..5];
let world = &s[6..11];

// v2
let hello = &s[..5];
let world = &s[6..];
```

### Structs
```rust
struct Rectangle {
    width: u32,
    height: u32,
}


impl Rectangle {
    fn width(&self) -> u32 {
        self.width
    }

    fn height(&self) -> u32 {
        self.height
    }

    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    let rect2 = Rectangle {
        width: 40,
        height: 60,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );

    println!(
        "The width of the rectangle is {} square pixels.",
        rect1.width()
    );

    rect1.can_hold(&rect2)
}
```

### Enumerations
Where structs give you a way of grouping together related fields and data, like a Rectangle with its width and height, enums give you a way of saying a value is one of a possible set of values.

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));
```

enums can also have diffeernt variants within
```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

You can also put structs inside an enum
```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

### Match example
```rust
#[derive(Debug)] // so we can inspect the state in a minute
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState)
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        },
        other => println!("This is how you catch all {:?}", other)
        // we can also use _ => to call all other if we don't need to use the value
    }
}
```

match vs if let
```rust
let mut count = 0;
match coin {
    Coin::Quarter(state) => println!("State quarter from {:?}!", state),
    _ => count += 1,
}

let mut count = 0;
match coin {
    Coin::Quarter(state) => println!("State quarter from {:?}!", state),
    _ => count += 1,
}
```
