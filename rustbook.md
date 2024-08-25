## snipets
```rust
let mut guess = String::new();
// read value
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
// and print it
println!("You wrote: {}", guess)


// match with destruct
match (player, opp) {
        // Ходы камня
        (Rock, Rock) => Draw,
        (Rock, Paper) => Lose,
        (Rock, Scissors) => Win,
        // Ходы бумаги
        (Paper, Rock) => Win,
        (Paper, Paper) => Draw,
        (Paper, Scissors) => Lose,
        // Ходы ножниц
        (Scissors, Rock) => Lose,
        (Scissors, Paper) => Win,
        (Scissors, Scissors) => Draw,
    }

// Another example
enum Temperature {
    Celsius(i32),
    Fahrenheit(i32),
}

fn main() {
    let temperature = Temperature::Celsius(35);
    match temperature {
        Temperature::Celsius(t) if t > 30 => println!("{}C is above 30 Celsius", t),
        Temperature::Celsius(t) => println!("{}C is below 30 Celsius", t),
        Temperature::Fahrenheit(t) if t > 86 => println!("{}F is above 86 Fahrenheit", t),
        Temperature::Fahrenheit(t) => println!("{}F is below 86 Fahrenheit", t),
    }
}
```
## Типы данных
```rust
// Кортежи
let tup: (i32, f64, u8) = (500, 6.4, 1);
// tup.1 - доступ к элементу по номеру

fn main() {
    let tup = (500, 6.4, 1);
    let (x, y, z) = tup; // Destruct
    println!("The value of y is: {y}");
}

// Массивы
let a: [i32; 5] = [1, 2, 3, 4, 5];
```
## Функции
```rust
fn add(a: i32, b: i32) -> i32 {
    a + b // без ";" так как это выражение
}
```
## Управляющие конструкции
```rust
let number = if a > b {
} else if {
} else {
}
```
## Циклы
```rust
let result = loop {
  break some_value;
  continue;
}

while number != 0 {
}

let a = [10, 20, 30, 40, 50];
for element in a {
    println!("the value is: {element}");
}

for number in (1..4).rev() {} // (1..=4) - include right value
```
## Ownership (владение)
```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward
} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope. Nothing special happens.
```
## Ссылки
```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```
## Изменяемая ссылка
Важное ограничение: может быть только одна
```rust
//  mutable reference
change(&mut s);
fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
## Cрезы
```rust
let s = String::from("hello world");
let hello = &s[0..5]; // equal &s[..5]
let world = &s[6..11]; // equal &s[6..]
let allS = &s[..]; // срез весго слова 

let a = [1, 2, 3, 4, 5];
let slice = &a[1..3]; // type: &[i32]
```
##  Структуры
```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32
}

impl Rectangle {
    // Типо конструктор квадратного прямоугольника
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }

    fn area(&self) -> u32 { // Сокращение от self: &Self
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width >= other.width && self.height >= other.height
    }
}

fn main() {
    let rectangle1 = Rectangle {width: 4,height: 5};
    let rectangle2 = Rectangle {width: 2,height: 3};
    let square = Rectangle::square(30);

    println!("Area of rectangle {:?} is {}", rectangle1, rectangle1.area());
    println!("Can rect1 hold rect2? {}", rectangle1.can_hold(&rectangle2));
    println!("Can rect1 hold square? {}", rectangle1.can_hold(&square));
}

// Синтаксис обновления структуры (user2 = копия (либо перемещение) user1 кроме email)
let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
```
### Кортежная структура (кортеж с именем)
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```
## Перечисления
```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));

// another example
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
```
### Option
[See doc here](https://doc.rust-lang.org/std/option/enum.Option.html)


