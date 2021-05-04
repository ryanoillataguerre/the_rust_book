# 6.1 Defining an Enum

- Enums store types that share the same root type

```rust
enum IpAddrKind {
	V4,
	V6
}
```

- You can then define functions that take an enum as an argument

```rust
fn route(ip_kind: IpAddrKind) {
	println!("IP Type: {}", ip_kind);
}

route(IpAddrKind::V4);
```

- You can use enums in structs

```rust
fn main() {
	enum IpAddrKind {
		V4,
		V6,
	}

	struct IpAddr {
		kind: IpAddrKind,
		address: String,
	}

	let home = IpAddr {
		kind: IpAddrKind::V4,
		address: String::from("127.0.0.1"),
	};

	let loopback = IpAddr {
		kind: IpAddrKind::V6,
		address: String::from("::1"),
	};
}
```

- More concisely:

```rust
fn main() {
	enum IpAddr {
		V4(String),
		V6(String),
	}

	let home = IpAddr::V4(String::from("127.0.0.1"));

	let loopback = IpAddr::V6(String::from("::1"));
}

```

- You can handle different amounts and types of data in different parameters of an enum

```rust
fn main() {
	enum IpAddr {
		V4(u8, u8, u8, u8),
		V6(String),
	}

	let home = IpAddr::V4(127, 0, 0, 1);

	let loopback = IpAddr::V6(String::from("::1"));
}
```

- These are the same

```rust
enum Message {
	Quit,
	Move { x: i32, y: i32 },
	Write(String),
	ChangeColor(i32, i32, i32),
}
```

```rust
struct QuitMessage; // unit struct
struct MoveMessage {
	x: i32,
	y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct
```

- You can also add `impl`s to enums, like structs

```rust
fn main() {
	enum Message {
		Quit,
		Move { x: i32, y: i32 },
		Write(String),
		ChangeColor(i32, i32, i32),
	}

	impl Message {
		fn call(&self) {
			// method body would be defined here
		}
	}

	let m = Message::Write(String::from("hello"));
	m.call();
}
```

- `Option<T>` is an enum, but it is not the same type as the value you give it

```rust
// Will not compile, because we're trying to add a Some type and an i8
fn main() {
	let x: i8 = 5;
	let y: Option<i8> = Some(5);

	let sum = x + y;
}
```

- In this case, you have to extract the value from `y` in order to use it, and need to handle each case where it either does or does not have a value

# 6.2 The match Control Flow Operator

```rust
enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => {
			println!("Lucky penny!");
			1
		},
		Coin::Nickel => 5,
		Coin::Dime => 10,
		Coin::Quarter => 25,
	}
}
```

```rust
fn main() {
	fn plus_one(x: Option<i32>) -> Option<i32> {
		match x {
			None => None,
			Some(i) => Some(i + 1),
		}
	}

	let five = Some(5);
	let six = plus_one(five);
	let none = plus_one(None);
}

```

- Rust also has a catchall `_` case

```rust
fn main() {
	let some_u8_value = 0u8;
	match some_u8_value {
		1 => println!("one"),
		3 => println!("three"),
		5 => println!("five"),
		7 => println!("seven"),
		_ => (),
	}
}

```

# 6.3 If let statements

- If let allows you to match on more than the type or a single parameter

```rust
#[derive(Debug)]
enum UsState {
	Alabama,
	Alaska,
	// --snip--
}

enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter(UsState),
}

fn main() {
	let coin = Coin::Penny;
	let mut count = 0;
	if let Coin::Quarter(state) = coin {
		println!("State quarter from {:?}!", state);
	} else {
		count += 1;
	}
}

```
