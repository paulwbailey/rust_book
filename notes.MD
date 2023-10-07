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
- String literal vs String: A string literal is a known value that is hardcoded into program. It is immutable. A String is a variable type like other languages. It can be mutable. String literals are saved in the stack and are faster.
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

### Collections

#### Vectors

Vectors are zero indexed

```rust
let v = vec![1, 2, 3, 4, 5];

// panicks
let does_not_exist = &v[100];

// returns None
let does_not_exist = v.get(100);

// iteratve over elements
let v = vec![100, 32, 57];
for i in &v {
    println!("{i}");
}

// iteratve over mutable elements. add 50 to each elements
// To change the value that the mutable reference refers to, we have to use the * dereference operator to get to the value in i before we can use the += operator. 
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

All types in a vector must be the same. We can use enums when types vary since each element would be that enum type
```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

#### Strings

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

// concatenate strings
let s = format!("{s1}-{s2}-{s3}");

//interate over string
for c in "abcd".chars() {
    println!("{c}");
}
```

#### Hash Maps (dictionaries)
```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// access element
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);

// loop over
for (key, value) in &scores {
    println!("{key}: {value}");
}

// add value if not exists
scores.entry(String::from("Green")).or_insert(50);
```

```rust
// count number of words using hash map
use std::collections::HashMap;

let text = "hello world wonderful world";

let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}

println!("{:?}", map);
```

By default, HashMap uses a hashing function called [SipHash](https://en.wikipedia.org/wiki/SipHash) that can provide resistance to Denial of Service (DoS) attacks involving hash tables1. This is not the fastest hashing algorithm available, but the trade-off for better security that comes with the drop in performance is worth it. If you profile your code and find that the default hash function is too slow for your purposes, you can switch to another function by specifying a different hasher.

### Error Handling
```rust
// force panic
panic!("forced panic");
```

```rust
// handle error
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}

// another way to handle
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")
        .expect("hello.txt should be included in this project");
}
```

The ? placed after a Result value is defined to work in almost the same way as the match expressions we defined to handle the Result values in Listing 9-6. If the value of the Result is an Ok, the value inside the Ok will get returned from this expression, and the program will continue. If the value is an Err, the Err will be returned from the whole function as if we had used the return keyword so the error value gets propagated to the calling code.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();

    File::open("hello.txt")?.read_to_string(&mut username)?;

    Ok(username)
}
```

```rust
// read file to string
use std::fs;
use std::io;

fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```
