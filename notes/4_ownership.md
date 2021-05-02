# 4.1 What is Ownership?

### Stack and Heap

- Stack is LIFO
- Heap is unordered, memallocator reserves a spot on the heap with a set amount of space and returns a pointer to that mem address

- Pushing/popping to/from the stack is faster than allocating or retrieving values from the heap

In Rust:

- When your code calls a function, the values passed into the function get pushed onto the stack. When the function call is over, those values get popped off the stack

Ownership manages minimizing the amount of duplicate data on the heap and cleaning up unused data efficiently

### Rules

- Each value in Rust has a variable that's called its owner
- There can only be one owner at a time
- When the owner goes out of scope, the value will be dropped (Rust calls the drop() function)

Example with strings

```rust
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

- This will error because once `s2` is defined, `s1` is dropped from the stack. So the value is actually ~moved~ to `s2`

- If we do want to deeply copy the heap data of the String, not just the stack data, we can use a method called `clone`

```rust
fn main() {
	let s1 = String::from("hello");
	let s2 = s1.clone();

	println!("s1 = {}, s2 = {}", s1, s2);
}
```

- However, shallow + deep cloning will work for integers because integers have a known size at compile time, so there's no difference between calling clone on an int and doing this:

```rust
fn main() {
	let x = 5;
	let y = x;

	println!("x = {}, y = {}", x, y);
}
```

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("hello"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// takes_and_gives_back will take a String and return one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

# 4.2 References and Borrowing

- `&` denotes a ~reference~ rather than passing the value itself into the new scope

```rust
fn main() {
	let s1 = String::from("hello");

	let len = calculate_length(&s1);

	println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
	s.len()
}
```

- In this case, we still can't change `s` inside `calculate_length` because that would be changing a reference

```rust
fn main() {
	let mut s = String::from("hello");

	change(&mut s);
}

fn change(some_string: &mut String) {
	some_string.push_str(", world");
}
```

- We can't have multiple mutable references to a particular piece of data in the same scope. Will get a compile error
- Can get around this by using mutations inside an expression (different scope)

```rust
fn main() {
	let mut s = String::from("hello");

	{
		let r1 = &mut s;
	} // r1 goes out of scope here, so we can make a new reference with no problems.

	let r2 = &mut s;
}
```

- You can't return a reference out of its scope, because it will be dropped at the end of the func and will reference nothing (null pointer)

```rust
fn main() {
	let reference_to_nothing = dangle();
}

fn dangle() -> &String {
	let s = String::from("hello");

	&s
}
// THIS WILL THROW AN ERROR
```

Instead, return the value itself

```rust
fn main() {
	let string = no_dangle();
}

fn no_dangle() -> String {
	let s = String::from("hello");

	s
}

```

# 4.3 The Slice Type

- Does not have ownership

```rust
fn first_word(s: &String) -> usize {
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

	// word still has the value 5 here, but there's no more string that
	// we could meaningfully use the value 5 with. word is now totally invalid!
}
```

**_Sidenote_**

```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
// slice is the same, can use the range syntax without 1st param if you want elements from the beginning
// You can also use 2.. to get from 2 to the end of the string
```

- Working solution:

```rust
n first_word(s: &String) -> &str {
	let bytes = s.as_bytes();

	for (i, &item) in bytes.iter().enumerate() {
		if item == b' ' {
			return &s[0..i];
		}
	}

	&s[..]
}
```

- Slices also apply to arrays

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];
```
