---
title: Rust
---

https://doc.rust-lang.org/book

## rustup
`rustup` is a command line tool for managing Rust versions and associated tools.
```bash
$ curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh

// macOS
$ xcode-select --install
```

## Hello, World!
```bash
$ touch hello_world.rs
```
```rust
// hello_world.rs

fn main() {
    println!("Hello, World!");
}
```
```bash
$ rustc hello_world.rs
$ ./hello_world
Hello, World!
```

- Use an `_` to separate multiple words in your filename.
- `main` function is the entry point of every Rust program.
- `println!` calls a Rust macro. 
- End the line with a `;`.

## Hello, Cargo!

Cargo is Rust’s build system and package manager.
```bash
# create a new cargo project
$ cargo new hello_cargo
$ cd hello_cargo

# check if the project is executable
$ cargo check

# run the project without binary output
$ cargo run
Hello, world!

# build the project and output a binary output
$ cargo build
$ ./target/debug/hello_cargo
Hello, world!

# build the project for release
$ cargo build --release
$ ./target/debug/hello_cargo
```
```toml
# Cargo.toml

[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

[dependencies]
```

## Variables and Constants
### Mutability
```rust
fn main() {
    // x is immutable by default
    let x = 5;

    // create mutable variable
    let mut x = 5;
    x = 6;

    // use uppercase with underscores between words for constants
    // compiler could do a limited set of operations at compile time
    const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
}

```
### Shadowing
```rust
fn main() {
    let x = 5;
    let x = x + 1;
    {
        let x = x * 2; // x is 12
    }
    // x is 6

    let spaces = "   "; // spaces is str "   "
    let spaces = spaces.len(); // spaces is int 3
}
```
The second variable shadows the first, taking any uses of the variable name for itself until either it is shadowed itself or the scope ends.
It is useful to reuse simpler variable names instead of creating new ones.

## Data Types
Rust is a statically typed language.

### Scalar Types

#### Integer
| Signed | Unsigned
|-|-|
|i8		|u8
|i16	|u16
|i32	|u32
|i64	|u64
|i128	|u128
|isize	|usize

|Literals |Example|
|-|-|
|Decimal		|98_222
|Decimal(u32)	|1_234u32
|Hex			|0xff
|Octal 			|0o77
|Binary			|0b1111_0000
|Byte (u8 only)	|b'A'

Rust panic at runtime in debug mode when overflow, but not in release mode.

#### Floating Point
```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
}
```

#### Boolean
```rust
fn main() {
    let t = true;
    let f: bool = false;
}
```

#### Character
```rust
fn main() {
    let c = 'z'; 
    let z: char = 'ℤ';
    let heart_eyed_cat = '😻';
}
```
- Use single quote for character literal. Able to store unicode.
- 4 bytes per character.

### Compound Types

#### Tuple
```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
	let (x, y, z) = tup;
	let first = tup.0;
	let u: () = ()
}
```
- Once declared, they cannot grow or shrink in size.
- `unit` is tuple with 0 values

#### Array
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
	let b: [i32; 5] = [1, 2, 3, 4, 5];
	let c: [3; 5]; // [3, 3, 3, 3, 3]
    let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

	let d = a[0]
}
```
- fix length, allocated on the stack.
- will panic if 

## Function
```rust
fn main() {
    println!("Hello, world!");

    another_function(5, 'c');
}

fn another_function(x: i32, unit_label: char) -> i32 {
	5
}
```
- snake case style.
- you can define the function in any order.


## Statements and Expressions

Statements are instructions that perform some action and do not return a value.
```rust
// statement
let x = 1 

fn foo() {
}
```

- Expressions evaluate to a resultant value
```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}"); // 4
}
```
- `x + 1` is an expression
- expression does not have a ';' at the end.
- Adding a ';' turn a expression into a statement.

## Comments
```rust
// Comments here
// Comments here
fn main() {
    // Comments here
    let lucky_number = 7; // Comments here
}
```

## Control Flow

### if
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

    let number = if condition { 5 } else { 6 };
    let number = if condition { 5 } else { "six" }; // compile error
}

```
- condition must be a `bool`
- `if` is an expression, and must have a single type

### loop
```rust
fn main() {
    let mut counter = 0;

    let result = 'label: loop {
        counter += 1;

        if counter == 10 {
            break 'label counter * 2;
        }

		continue 'label
    };
}
```
- `loop` is an expression
- you can assign label for inner loop to `break` or `continue`
- `break;` equals `break ();`

### while
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
- no break value in while

### for
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }

    for number in (1..4).rev() {
        println!("{number}!");
    }
}
```
- no break value in for

## Ownership

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.
