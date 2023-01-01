---
title: Rust
---

- https://doc.rust-lang.org/book

## 1. `rustup`
`rustup` is a command line tool for managing Rust versions and associated tools.
```sh
$ curl --proto '=https' --tlsv1.3 https://sh.rustup.rs -sSf | sh

# MacOS
$ xcode-select --install
```

## 2. Hello, World!
```bash
# Use an `_` to separate multiple words in your filename.
$ touch hello_world.rs
```
```rust
// hello_world.rs

// `main` function is the entry point of every Rust program.
fn main() {
    // `println!` calls a Rust macro. 
    // End the line with a `;`.
    println!("Hello, World!");
}
```
```bash
$ rustc hello_world.rs
$ ./hello_world
Hello, World!
```

## 3. `cargo`

`cargo` is Rust’s build system and package manager
```bash
# Create a new cargo project.
$ cargo new hello_cargo
$ cd hello_cargo

# Check if the project is executable.
$ cargo check

# Run the project without binary output.
$ cargo run
Hello, world!

# Build the project and output a binary output.
$ cargo build
$ ./target/debug/hello_cargo
Hello, world!

# Build the project for release.
$ cargo build --release
$ ./target/release/hello_cargo
```
```toml
# Cargo.toml

[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

[dependencies]
```

## 4. Variables and Constants
### 4.1 Mutability
```rust
fn main() {
    // `foo` is immutable by default.
    let foo = 5;

    // Create a mutable variable.
    let mut bar = 5;
    bar = 6;

    // Use uppercase with underscores between words for constants.
    // Compiler could do a limited set of operations at compile time.
    const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
}

```
### 4.2 Shadowing
It is useful to reuse simpler variable names instead of creating new ones.
```rust
fn main() {
    let foo = 5;
    // The second variable shadows the first. 
    let foo = foo + 1;
    {
        let foo = foo * 2; // foo is 12

		// The scope ends.
    }
    // `foo` is 6

    let bar = "   "; // `bar` is str "   "
    let bar = bar.len(); // `bar` is int 3
}

```

## 5. Data Types

### 5.1 Integer (scalar type)
```rust
fn main() {
    // Signed
    let num = 123; // the default type is `i32`.
    let num_8: i8 = 123;
    let num_16: i16 = 123;
    let num_32: i32 = 123;
    let num_64: i64 = 123;
    let num_128: i128 = 123;
    // It depends on the architecture of your computer.
    let num_size: isize = 123;

    // Unsigned
    let un_num_8: u8 = 123;
    let un_num_16: u16 = 123;
    let un_num_32: u32 = 123;
    let un_num_64: u64 = 123;
    let un_num_128: u128 = 123;
    // It depends on the architecture of your computer.
    let un_num_size: usize = 123;

    // Literals
    // You can put `_` any where in a literal.
	let decimal = 123_456;
	let decimal_u32 = 123_456u32;
	let hex = 0xabc;
	let octal = 0x123;
	let binary = 0b1111_0000;
	let by = b'A'; // The type must be `u8`.
}
```

### 5.2 Floating Point (scalar type)
```rust
fn main() {
    let x = 2.0; // `f64`
    let y: f32 = 3.0; // `f32`
}
```

### 5.3 Boolean (scalar type)
```rust
fn main() {
    let t = true;
    let f: bool = false;
}
```

### 5.4 Character (scalar type)
```rust
fn main() {
    // Use single quote for character literal.
	// Able to store unicode.
    // 4 bytes per character.
    let c = 'z'; 
    let z: char = 'ℤ';
    let heart_eyed_cat = '😻';
}
```

### 5.5 Tuple (compound types)
```rust
fn main() {
    // Cannot grow or shrink in size.
    let tup: (i32, f64, u8) = (500, 6.4, 1);
	let (x, y, z) = tup;
	let first = tup.0;

    // `unit` is tuple with 0 values.
	// A function that doesn't have `return` will automatically return `unit`.
	let u: () = ()
}
```

### 5.6 Array (compound types)
```rust
fn main() {
    // Arrays are fix length, allocated on the stack.
    let a = [1, 2, 3, 4, 5];
	let b: [i32; 5] = [1, 2, 3, 4, 5];
	let c: [3; 5]; // [3, 3, 3, 3, 3]
    let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

	let d = a[0]
}
```

## 6. Function
```rust
fn main() {
    println!("Hello, world!");

    // Use snake case style.
    another_function(5, 'c');
}

// You can define the function in any order.
fn another_function(x: i32, unit_label: char) -> i32 {
	5
}
```

## 7. Statements and Expressions

**Statements** are instructions that perform some action and do not return a value.
```rust
// statement
let x = 1 

fn foo() {
}
```

**Expressions** evaluate to a resultant value
```rust
fn main() {
    let y = {
        let x = 3;
		// expressions doesn't have a `;` at the end.
        // Adding a ';' turn a expression into a statement.
        x + 1
    };

    println!("The value of y is: {y}"); // 4
}
```

## 8. Comments
```rust
// Comments here.
// Comments here.
fn main() {
    // Comments here
    let foo = "bar"; // Comments here.
}
```

## 9. Control Flow

### 9.1 if (expression)
```rust
fn main() {
    let number = 6;

    // condition must be a `bool`.
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
	// It's a compile error because the return value must have the same types.
    let number = if condition { 5 } else { "six" }; 
}

```

### 9.2 loop (expression)
```rust
fn main() {
    let mut counter = 0;

    // You can assign label for inner loop to `break` or `continue`.
    let result = 'label: loop {
        counter += 1;

        if counter == 10 {
            break 'label counter * 2;
        }

		continue 'label
    };

	// `break;` equals `break ();`.
}
```

### 9.3 while (expression but only return `()`) 
```rust
fn main() {
    let mut number = 3;

    // No break value in `while`.
    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

### 9.4 for (expression but only return `()`)
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    
    // No break value in `for`.
    for element in a {
        println!("the value is: {element}");
    }

    for number in (1..4).rev() {
        println!("{number}!");
    }
}
```

## Ownership

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

### Variable Scope and Memory Allocation
```rust
fn main() {
    {  
        // foo is valid after this line
        let foo = String::from("Hello, world!");

        // move occurs because `foo` has type `String`, which does not implement the `Copy` trait
        // foo is no longer usable until reasign with value
        let bar = foo


		let baz = 123
		// direct copy baz to quz
		let qux = baz
		// baz is still abailable here


		// this scope is now over and call drop(bar)
		// `foo` is not the owner of the string so it doesn't need to be released
		// `baz`, `quz` don't need to be released because it's value is not in heap
    }
}
```
- `String` implement the `Drop` trait.
- scalar values or nothing that requires allocation or is some form of resource can implement `Copy`. Tuples, if they only contain types that also implement `Copy`


