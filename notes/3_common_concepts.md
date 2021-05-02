# 3.1 Variables and Mutability

- Have to prefix mutable variables with `mut`

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

- Constants have to be strictly typed

```rust
const MAX_POINTS: u32 = 100_000;
```

### Shadowing

```rust
fn main() {
	let x = 5;

	let x = x + 1;

	let x = x * 2;

	println!("The value of x is: {}", x);
}

// The value of x is: 12
```

```rust
let spaces = "    ";
let spaces = spaces.len();
// Works: 4
```

```rust
let mut spaces = "    ";
spaces = spaces.len();
// Compile error
```

- Error because `mut` does not let you change the variable's type

# 3.2 Data Types

- Rust is statically typed: It must know the types of all variables at compile time

- Scalar types: `integers`, `floating-point numbers`, `Booleans`, `characters`

### Integers

- Types are e.g. `u32` where `u` vs `i` is unsigned vs signed and `32` is # bits of space the number takes up
  - Each signed variant can store numbers from `-(2^n-1) to 2^n-1 - 1` inclusive

### Floating Point Types

- `f` instead of `i` or `u`

### Booleans

- `let f: bool = false;`

### Characters

- `let c = 'Z';`

### Tuples

- `let tup: (i32, f64, u8) = (500, 5.4, 1);`

```rust
fn main() {
	let tup = (500, 6.4, 1);
	let (x, y, z) = tup;

	println!("The value of y is: {}", y);
}
// 6.4
```

```rust
fn main() {
	let x: (i32, f64, u8) = (500, 6.4, 1);

	let five_hundred = x.0;

	let six_point_four = x.1;

	let one = x.2;
}
```

### Arrays

```rust
fn main() {
	let a = [1, 2, 3, 4, 5];
}
```

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

- Create an array of length 5 with all 3s: `let a = [3; 5];`

### Functions

```rust
fn main() {
	let x = 5;

	let y = {
		let x = 3;
		x + 1
	};

	println!("The value of y is: {}", y);
}
```

- y evaluates the expression inside of it and returns 4
- Expressions create new scopes

- Functions with return values

```rust
fn main() {
	let x = plus_one(5);

	println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
	x + 1
}
```

- `;` changes the expression to a statement, which would give an error in this case saying no tail or return expression

### Control Flow

```rust
fn main() {
	let number = 3;

	if number {
		println!("number was three");
	}
}
```

- This will resolve to an error
  - `if` expressions need to always resolve to a Boolean

```rust
fn main() {
	let number = 3;

	if number != 0 {
		println!("number was something other than zero");
	}
}
```

- Using if in a let statement

```rust
fn main() {
	let condition = true;
	let number = if condition { 5 } else { 6 };

	println!("The value of number is: {}", number);
}
```

- Can't use different types

```rust
fn main() {
	let condition = true;

	let number = if condition { 5 } else { "six" };

	println!("The value of number is: {}", number);
}
```

- This will error

### Loops

- Example basic loop in `guessing_game`

- Return a value from a loop by breaking with an expression

```rust
fn main() {
	let mut counter = 0;

	let result = loop {
		counter += 1;

		if counter == 10 {
			break counter * 2;
		}
	};

	println!("The result is {}", result);
}
```

- While loops

```rust
fn main() {
	let mut number = 3;

	while number != 0 {
		println!("{}!", number);

		number -= 1;
	}

	println!("LIFTOFF!!!");
}
```

- Looping through a collection with `for`

```rust
fn main() {
	let a = [10, 20, 30, 40, 50];

	for element in a.iter() {
		println!("the value is: {}", element);
	}
}
```

- Using a `Range` type

```rust
fn main() {
	for number in (1..4).rev() {
		println!("{}!", number);
	}
	println!("LIFTOFF!!!");
}
```
