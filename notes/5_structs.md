# 5.1 Defining and Instantiating Structs

- Structs are basically JS classes

```rust
struct User {
	username: String,
	email: String,
	sign_in_count: u64,
	active: bool,
}
```

```rust
fn main() {
	let mut user1 = User {
		email: String::from("someone@example.com"),
		username: String::from("someusername123"),
		active: true,
		sign_in_count: 1,
	};

	user1.email = String::from("anotheremail@example.com");
}
```

- You can return structs as types from functions

```rust
fn build_user(email: String, username: String) -> User {
	User {
		email: email,
		username: username,
		active: true,
		sign_in_count: 1,
	}
}
```

- Spread operator in Structs

```rust
let user2 = User {
	email: String::from("another@example.com"),
	username: String::from("anotherusername567"),
	..user1
};
```

Tuple Structs

```rust
fn main() {
	struct Color(i32, i32, i32);
	struct Point(i32, i32, i32);

	let black = Color(0, 0, 0);
	let origin = Point(0, 0, 0);
}

```

# 5.2 An Example Program Using Structs

[Project](../structs/src/main.rs)

- Have to add `#[derive(Debug)]` to top of file to use debug print notation:
  - `println!("rect1 is {:?}", rect1)` FOR STRUCTS TO PRINT
  - `println!("rect1 is {:#?}", rect1)` formats structs more cleanly

# 5.3 Methods

[Project](../methods/src/main.rs)
