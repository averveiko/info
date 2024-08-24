## snipets
```rust
let mut guess = String::new();
// read value
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");
// and print it
println!("You wrote: {}", guess)
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
```
## Циклы
```rust
loop {
  break;
  continue;
}
```
