# Rust

## Variables and Mutability

- Default variables are immutable
  ```rust
  let x = 10;
  ```
- Can make the above example mutable by adding `mut`
 ```rust
 let mut x: i32 = 20;
 x= 30;
 ```
- **const**: constants are values that are bound to a name and are not allowed to change
 ```rust
 const SERVER_NUMBER: u8 = 4;
 ```
 - Always immutable, so not allowed to use `mut` with const
 - Constants can be declared in any scope
 - Constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

- **Shadowing**: you can declare a new variable with the same name as a previous variable. This feature is often used when you want to convert a value from one type to another type.

 ```rust
    let x = 5;
    let x = x + 1; //x is 6
    {
    	let x = x*2 // x is 12
    }
 ```
 ```rust
     let spaces = "   "; // type is string
     let spaces = spaces.len(); // type is number
 ```
 > we’re not allowed to mutate a variable’s type:
```rust
let mut sapce= "   ";
let space= space.len(); // compile failed
```


## Data Types

 **Rust** is a statically typed language, which means that it must know the types of all variables at compile time.

### Scalar Types
A scalar type represents a single value:

- Integers

| Length | Signed | Unsigned |
|--------|--------|--------|
|8-bit   |   	i8|    	 u8|
|16-bit	|i16	|u16|
|32-bit	|i32	|u32|
|64-bit	|i64	|u64|
|128-bit|i128|	u128|
|arch	|isize|	usize   |

- Floating-point numbers
  Rust’s floating-point types are `f32` and `f64`

- Booleans
  The Boolean type in Rust is specified using `bool`

- Characters
  Rust’s char type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII.
  >char literals with single quotes, as opposed to string literals

### Compound Types
Compound types can group multiple values into one type. Rust has two primitive compound types:
- Tuples
  - Tuples have a fixed length: once declared, they cannot grow or shrink in size.
  - Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same.
  ```rust
	let tup: (i32, f64, u8) = (500, 6.4, 1);
  ```

- Arrays
  - Every element of an array must have the same type.
  - Arrays in Rust have a fixed length.
  - Arrays are useful when you want your data allocated on the stack rather than the heap
  - You write an array’s type using square brackets with the type of each element, a semicolon, and then the number of elements in the array
  ```rust
	let a: [i32; 5] = [1, 2, 3, 4, 5];
  ```

## Functions

Function bodies are made up of a series of statements optionally ending in an expression.

> Rust is an expression-based language

- Statements are instructions that perform some action and do not return a value. Creating a variable and assigning a value to it with the let keyword is a statement.

- Expressions evaluate to a resulting value. Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, and it will then not return a value.

We don’t name return values, but we must declare their type after an arrow (->)
```rust
fn five() -> i32 {
    5
}
```

- The return value of the function is synonymous with the value of the final expression in the block of the body of a function.
- You can return early from a function by using the return keyword and specifying a value, but most functions return the last expression implicitly
  ```rust
  fn fib(n:u32) -> u32 {
    if n <=1 {
      return n;
    }
    fib(n - 1) + fib(n - 2)
  }
  ```
  - The type of n is specified as u32
  - The type of result must be u32


## Comment

In Rust, the idiomatic comment style starts a comment with two slashes, and the comment continues until the end of the line. For comments that extend beyond a single line, you’ll need to include // on each line, like this:
```
// So we’re doing something complicated here.
// multiple lines of comments to do it! Whew!
// explain what’s going on.
```

## Control FLow
The most common constructs that let you control the flow of execution of Rust code are if expressions and loops.

### if Expressions
Because if is an expression, we can use it on the right side of a let statement to assign the outcome to a variable:
```rust
let condition = true;
let number = if condition { 5 } else { 6 };
```

Rust will not automatically try to convert non-Boolean types to a Boolean. You must be explicit and always provide if with a Boolean as its condition.
```rust
let status= true;

if status {
	println!("Request is {}", status)
}
```

You can use multiple conditions by combining if and else in an else if expression. For example:
```rust
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
```

If you don’t provide an else expression and the condition is false, the program will just skip the if block and move on to the next bit of code.

```rust
let number = 3;

if number {
    println!("number was three");
}
```

### Repetition with Loops

Rust has three kinds of loops:
- loop
- while
- for



#### loop
The loop keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop.
```rust
loop {
    println!("again!");
}
```
  - Rust also provides a way to break out of a loop using code. You can place the `break` keyword within the loop to tell the program when to stop executing the loop.
  - `continue` in a loop tells the program to skip over any remaining code in this iteration of the loop and go to the next iteration.
  - If you have loops within loops,You can optionally specify a loop label on a loop that we can then use with break or continue to specify that those keywords apply to the labeled loop instead of the innermost loop.
  ```rust
  let mut count = 0;
  	//here
    'counting_up: loop {
        println!("count = {}", count);
        let mut remaining = 10;

        loop {
            println!("remaining = {}", remaining);
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up; //here
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {}", count);
 ```

- **Returning Values from Loops**: you can add the value you want returned after the `break` expression.
```rust
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2; // here
        }
    };

    println!("The result is {}", result);
```

#### Conditional Loops with `while`
A program will often need to evaluate a condition within a loop. While the condition is true, the loop runs. When the condition ceases to be true, the program calls `break`, Rust has a built-in language construct for it, called a `while` loop.

```rust
let mut number = 3;

while number != 0 {
    println!("{}!", number);

    number -= 1;
}
```

#### Looping Through a Collection with `for`
 you can use a `for` loop and execute some code for each item in a collection.
```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("the value is: {}", element);
}
```
`rev()` : to reverse the range:
```rust
for number in (1..4).rev() {
    println!("{}!", number);
}
println!("LIFTOFF!!!");
```

## Examples - Part 1


### 1- Generate the nth Fibonacci number.
```rust
use std::io;

fn main() {
    let mut input = String::new();

    println!("Type a number:");

    io::stdin().read_line(&mut input).expect("Try again later!");

    let input: u32 = input.trim().parse().expect("Please inter a number");

    let res:u32 = fib(input);

    println!("Fib = {}", res);
}

// n <=1 return

fn fib(n:u32) -> u32 {
    if n <=1 {
      return n;
    }

    fib(n - 1) + fib(n - 2)
}
```

### 2- Convert temperatures between Fahrenheit and Celsius.

```rust
use std::io;

fn main() {
    let rate = 1.80;

    loop{
        let mut temp_type = String::new();
        println!("\n 0- exit \n 1- Celsius to Fahrenheit \n 2- Fahrenheit to Celsius");
        println!("1 or 2:");
        io::stdin().read_line(&mut temp_type).expect("failed to read!");

        println!("Type is {}", temp_type);

        let temp_type:u32 = temp_type.trim().parse().expect("Please type a number!");

        if temp_type == 1 {
            let current_temp = get_input();
            let fahrenheit_temp = (current_temp * rate) + 32.00;
            println!("{} temp is: {}", "F", fahrenheit_temp );
        }else if temp_type== 2 {
            let current_temp = get_input();
            let celsius_temp: f32= (current_temp - 32.00) / rate;
            println!("{} temp is: {}", "C", celsius_temp );
        } else {
            println!("Bye!");
            break;
        }
    }
}

fn get_input()->f32{
    let mut temp = String::new();
    println!("Type your temp:");
    io::stdin().read_line(&mut temp).expect("fail!");
    let temp: f32 = temp.trim().parse().expect("Please type the temp!");

    temp
}
```

### 3- Are you playing banjo?

Create a function which answers the question "Are you playing banjo?".
If your name starts with the letter "R" or lower case "r", you are playing banjo!

The function takes a name as its only argument, and returns one of the following strings:
``` bash
name + " plays banjo"
name + " does not play banjo"
```

Solution:
```rust
fn are_you_playing_banjo(name: &str) -> String {
    let first_char = name
    .chars()
    .next()
    .unwrap()
    .to_lowercase()
    .to_string();

    let mut result = format!("{} does not play banjo", name);

    if first_char == "r" {
       result = format!("{} plays banjo", name)  ;
    }

    result
}
```

### 4- If you can't sleep, just count sheep!!
[Codewar's Source](https://www.codewars.com/kata/5b077ebdaf15be5c7f000077/rust)

Given a non-negative integer, 3 for example, return a string with a murmur: "1 sheep...2 sheep...3 sheep...". Input will always be valid, i.e. no negative integers.

Solution:

```rust
fn count_sheep(n: u32) -> String {
    let mut result = String::new();

    for num in 0..n {
        result = format!("{}{} sheep...", result, num+1);
    }

    result
}
```

### 5- Convert boolean values to strings 'Yes' or 'No'.
[Codewar's Source](https://www.codewars.com/kata/53369039d7ab3ac506000467/rust)

```rust
fn bool_to_word(value: bool) -> &'static str {
    if value {
       return "Yes"
    }
    "No"
}
```

### 6- Sum of positive
[Codewar's Source](https://www.codewars.com/kata/5715eaedb436cf5606000381/train/rust)
```rust
fn positive_sum(slice: &[i32]) -> i32 {
    let mut res:i32 = 0;
    for num in slice {
        if num < &0 {continue} else {
            res += num;
        }
    }
    res
}
```

## Ownership
### Stack and Heap

- Both the stack and the heap are parts of memory available to your code to use at runtime.

- The stack stores values in the order it gets them and removes the values in the opposite order.

- Adding data is called pushing onto the stack, and removing data is called popping off the stack.

- All data stored on the stack must have a known, fixed size.
- Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

- The heap is less organized: when you put data on the heap, you request a certain amount of space. The memory allocator finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location. This process is called allocating on the heap and is sometimes abbreviated as just allocating. Pushing values onto the stack is not considered allocating. Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack, but when you want the actual data, you must follow the pointer

- Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a place to store new data; that location is always at the top of the stack. Comparatively, allocating space on the heap requires more work, because the allocator must first find a big enough space to hold the data and then perform bookkeeping to prepare for the next allocation.

- Accessing data in the heap is slower than accessing data on the stack because you have to follow a pointer to get there.

### Ownership Rules

1. Each value in Rust has a variable that’s called its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

### Memory and Allocation

In Rust, the memory is automatically returned once the variable that owns it goes out of scope. when a variable goes out of scope, Rust automatically calls the drop function and cleans up the heap memory for that variable.

#### Ways Variables and Data Interact: Move


```rust
let s1 = String::from("hello");
let s2 = s1;

// s1 invalidated, infact s1 moved to s2. and s2 is valid now.
println!("{}, world!", s1);
```

You’ll get an error because Rust prevents you from using the invalidated reference.
> Rust invalidates the first variable, instead of calling it a shallow copy, it’s known as a move.

Rust will never automatically create “deep” copies of your data. Therefore, any automatic copying can be assumed to be inexpensive in terms of runtime performance.

#### Ways Variables and Data Interact: Clone

If we do want to deeply copy the heap data of the String, not just the stack data, we can use a common method called `clone`

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```
#### Stack-Only Data: Copy
```rust
fn main() {
    let x = 5;
    let y = x;
	// x and y are valid, because thier type are interger.
    // simple scalar values store on the `stack`
    println!("x = {}, y = {}", x, y);
}
```
The reason is that types such as integers that have a known size at compile time are stored entirely on the `stack`, so copies of the actual values are quick to make.

Rust has a special annotation called the `Copy` trait that we can place on types that are stored on the stack like integers are.

You can check the documentation for the given type to be sure, but as a general rule, any group of simple scalar values can implement Copy, and nothing that requires allocation or is some form of resource can implement Copy. Here are some of the types that implement Copy:

- All the integer types, such as u32.
- The Boolean type, bool, with values true and false.
- All the floating point types, such as f64.
- The character type, char.
- Tuples, if they only contain types that also implement Copy. For example, (i32, i32) implements Copy, but (i32, String) does not.


### Ownership and Functions
Passing a variable to a function will move or copy, just as assignment does.
```rust
fn main() {
    // s comes into scope
    let s = String::from("hello");

     // s's value moves into the function...
     takes_ownership(s);
      // ... and so is no longer valid here

     // x comes into scope
     let x = 5;

     // x would move into the function,
     // but i32 is Copy, so it's okay to still
     // use x afterward
     makes_copy(x);

}
// Here, x goes out of scope, then s.
// But because s's value was moved, nothing special happens.



fn takes_ownership(some_string: String) {
// some_string comes into scope
println!("{}", some_string);
}
// Here, some_string goes out of scope and `drop` is called.
// The backing memory is freed.

fn makes_copy(some_integer: i32) {
	// some_integer comes into scope
    println!("{}", some_integer);
}
// Here, some_integer goes out of scope. Nothing special happens.

```

#### Return Values and Scope
```rust
fn main() {
    let mut s = String::from("Hello");
    s.push_str(", Jesi");


    // s moved to `calc_len` function.
    // return multiple values using a tuple
    let (s2, len) = calc_len(s);
    // s is invalidated now.
    // println!("{}", s); ->  throw a compile-time error. because s is invalidated.

    // clone s2 into s3.
    let s3= s2.clone();

    println!("s2={}, len={}", s2, len)
}


fn calc_len(s: String)-> (String, usize){
    let length = s.len();
    (s, length)
}
```