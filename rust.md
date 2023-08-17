# Rust conspect

## Snippets
```rust
// Чтение файла
use std::fs;

let read_result = fs::read_to_string(FILE_PATH);
    let content = match read_result {
        Ok(str) => str,
        Err(_) => {
            println!("Error reading file {}", FILE_PATH);
            std::process::exit(1);
        }
    };

// Использование zip
use std::iter::zip;

let xs = [1, 2, 3];
let ys = [4, 5, 6];

let mut iter = zip(xs, ys);

assert_eq!(iter.next().unwrap(), (1, 4));
assert_eq!(iter.next().unwrap(), (2, 5));
assert_eq!(iter.next().unwrap(), (3, 6));
assert!(iter.next().is_none());
```


```rust
fn gcd(mut n: u64, mut m: u64) -> u64 {
  assert!(n != 0 && m != 0);
  while  m != 0 {
    if  m < n {
      let t = m;  // local variable
      m = n;
      n = t;
    }
    m = m % n;
  }
  n
}

#[test] // это атрибут
fn test_gcd() {
  assert_eq!(gcd(14, 15), 1);
  assert_eq!(gcd(2 * 3 * 5 * 11 * 17, 3 * 7 * 11 * 13 * 19), 3 * 11);
}
```

```rust
// i32 – 32-разрядное целое со знаком, u8 – 8-разрядное целое без знака
// Типы isize и usize соответствуют целым размерам указателя со знаком и без знака: 32 разряда на 32-разрядной платформе и 64 – на 64-разрядной.
// f32, f64 aka float и double в C и C++

// любой блок, заключенный в фигурные скобки, может выступать в роли выражения:
{
  x.cos()
}

u64::from_str(&arg).expect("error parsing argument") // Распарсить, либо вернуть ошибку

(char,u8,i32) // Кортеж, допускаются различные типы ('%',0x7f,-1)

() // «Единичный» (пустой) тип

// Структура с именованными полями S {x:120.0,y:209.0}
struct S {
  x:f32,
  y:f32
}

// Кортежеподобная структура T(120,'X')
struct T (i32,char);

// Структура, подобная единичному типу, не имеет полей
struct E;

// Перечисление, алгебраический тип данных Attend::Late(5),Attend::OnTime
enum Attend {
  OnTime,
  Late(u32)
}

// Бокс – владеет указателем на значение в куче Box::new(Late(15))
Box<Attend>

// Разделяемая и изменяемая ссылка – не владеющие памятью указатели, которые не должны существовать дольше, чем объекты, на которые указывают &s.y, &mut v
&i32, &mut i32

// Строка в кодировке UTF-8, размер динамически изменяется "ラーメン:ramen".to_string()
String

// Ссылка на str, не владеющий памятью указатель на текст в кодировке UTF-8 "そば:soba", &s[0..12]
&str

// Массив фиксированной длины, все элементы одного типа [1.0, 0.0, 0.0, 1.0], [b' '; 256]
[f64;4], [u8;256]

// Вектор переменной длины, все элементы одного типа vec![0.367, 2.718, 7.389]
Vec<f64>

// Ссылка на срез: ссылка на участок массива, состоящая из указателя и длины &v[10..20], &mut a[..]
&[u8], &mut[u8]

// Объект характеристики, ссылка на любое значение, реализующее заданный набор методов value as &Any, &mut file as &mut Read
&Any, &mutRead

// Указатель на функцию i32::saturating_add
fn(&str, usize) -> isize

// Замыкание |a,b| a*a + b*b
(у замыкания нет явной формы)

// Целые числа имеют методы
assert_eq!(2u16.pow(4), 16);            //возведение в степень
assert_eq!((-4i32).abs(), 4);           //абсолютная величина
assert_eq!(0b101101u8.count_ones(), 4); //счетчик единиц

// Типы с плавающей точкой также имеют методы
assert_eq!(5f32.sqrt() * 5f32.sqrt(), 5.); //точно 5.0, в соответствии с IEEE
assert_eq!(-1.01f64.floor(), -1.0);
assert!((-1. / std::f32::INFINITY).is_sign_negative());

// Явное преобразование типов
i as f64
x as i32

// Некоторые методы типа Char
assert_eq!('*'.is_alphabetic(), false);
assert_eq!('β'.is_alphabetic(), true);
assert_eq!('8'.to_digit(10), Some(8));
assert_eq!('ಠ'.len_utf8(), 3);
assert_eq!(std::char::from_digit(2, 10), Some('2'));

// Кортежи
let t = ("Brazil", 1985, 'A') // (&str, i32, Char)
t.0, t.1, t.2
// Сигнатура метода деления строки на 2: fn split_at(&self, mid: usize) -> (&str, &str);
let text = "I see the eigenvalue in thine eye";
let (head, tail) = text.split_at(21); // Результат можно присвоить 2м переменным
assert_eq!(head, "I see the eigenvalue ");
assert_eq!(tail, "in thine eye");
```

## Указательные типы
```rust
// Ссылки
&String – ссылка на значение типа String

// Выражение &x порождает ссылку на x; в Rust говорят, что оно «заимствует ссылку на x»
// Если имеется ссылка r, то выражение *r дает значение, на которое указывает r
// &T – неизменяемая ссылка, как constT* в C;&mut T – изменяемая ссылка, как T* в C.

// Боксы
let t = (12, "eggs");
let b = Box::new(t);  //выделить память для кортежа из кучи
```

## Массивы

```rust
let mut chaos = [3, 5, 4, 1, 2];
chaos.sort();
chaos.reverse();
```

## Векторы

```rust
let mut v = Vec::new(); // Создать пустой вектор

let mut v = vec![2, 3, 5, 7];
assert_eq!(v.iter().fold(1, |a, b| a * b), 210);

v.push(11);
v.push(13);
assert_eq!(v.iter().fold(1, |a, b| a * b), 30030);

// Вставить в позицию 3 элемент 35.
v.insert(3, 35);
// Удалить элемент в позиции 1.
v.remove(1)

// Построить вектор из значений, порождаемых итератором:
let v: Vec<i32> = (0..5).collect(); // Нужно явно указать тип, так как collect умеет строить разные коллекции
assert_eq!(v, [0, 1, 2, 3, 4]);

// Если заранее известно, сколько элементов будет храниться в векторе, то вместо функции Vec::new можно вызвать функцию Vec::with_capacity
let mut v = Vec::with_capacity(2);
assert_eq!(v.len(), 0);
assert_eq!(v.capacity(), 2);

let mut v = vec!["carmen", "miranda"];
assert_eq!(v.pop(), Some("miranda"));
assert_eq!(v.pop(), Some("carmen"));
assert_eq!(v.pop(), None);
```

## Срезы
Срез, записываемый в виде [T] без указания длины, – это участок массива или вектора. Поскольку длина среза может быть любой, они не хранятся в переменных и не передаются в виде аргументов функции. Срезы всегда передаются по ссылке. Ссылка на срез является толстым указателем, она занимает два слова: указатель на первый элемент среза и количество элементов в нем.
```rust
let v: Vec<f64> = vec![0.0,  0.707,  1.0,  0.707];
let a: [f64; 4] =     [0.0, -0.707, -1.0, -0.707];

let sv: &[f64] = &v;
let sa: &[f64] = &a;

print(&v[0..2]); // напечатать первые два элемента v
print(&a[2..]); // напечатать элементы a, начиная с a[2]
print(&sv[1..3]); // напечатать v[1] и v[2]
```
## Строковые литералы
```rust
let speech = "\"Ouch!\" said the well.\n";

// Сырые строки, последовательности не распазнаются
let default_win_install_path = r"C:\Program Files\Gorillas";
let pattern = Regex::new(r"\d+(\.\d+)*");

println!(r###"
      Эта простая строка начинается последовательностью 'r###"'.
      Поэтому она заканчивается, только когда встретится кавычка('"'),
      за которой следуют три знака решетки('###'):
"###);
```

## Байтовые строки
Строковый литерал с префиксом b называется байтовой строкой. Она представляет собой срез значений типа u8 (т. е. байтов), а не Юникод-текст.
```rust
let method = b"GET";
assert_eq!(method, &[b'G', b'E', b'T']);

// Метод .len() типа String или &str возвращает длину строки, измеряемую в байтах, а не в символах:
assert_eq!("ಠ_ಠ".len(), 7);
assert_eq!("ಠ_ಠ".chars().count(), 3);
```

## String
```rust
let error_message = "too many pets".to_string();
// Макрос format!() работает как println!(), но вместо вывода в stdout возвра-щает новую строку, при этом в конец не добавляется знак новой строки
assert_eq!(format!("{}°{:02}′{:02}′′N", 24, 5, 23), "24°05′23′′N".to_string());

// У массивов, срезок и векторов есть два метода, .concat() и .join(sep), которые образуют новый объект String из нескольких строк:
let bits = vec!["veni", "vidi", "vici"];
assert_eq!(bits.concat(), "venividivici");
assert_eq!(bits.join(", "), "veni, vidi, vici");

// Использование
assert!("ONE".to_lowercase() == "one");
assert!("peanut".contains("nut"));
assert_eq!("ಠ_ಠ".replace("ಠ", "0"), "0_0");
assert_eq!("    clean\n".trim(), "clean");
for word in "veni, vidi, vici".split(", ") {
  assert!(word.starts_with("v"));
}
```

## Владение, move, etc

```rust
struct Person {
  name: Option<String>,
  birth: i32
}

let mut composers = Vec::new();
composers.push(Person { name: Some("Palestrina".to_string()), birth: 1525 });

// Следующее предложение недопустимо:
// let first_name = composers[0].name;

// но можно так
let first_name = std::mem::replace(&mut composers[0].name, None);
assert_eq!(first_name, Some("Palestrina".to_string()));
assert_eq!(composers[0].name, None);
// или более кратко:
let first_name = composers[0].name.take();

// По дефолту пользовательские структуры некопируемы, но это можно (и нужно) исправить (если  все  поля  структуры  имеют  копируемый тип)
#[derive(Copy, Clone)]
struct Label { number: u32 }
// объявление типа копируемым влечет серьезные обязательства со стороны его автора: если в последствии тип понадобится сделать некопируемым, то код, в котором он использовался, возможно, придется модифицировать
```
## Rc и ARc: совместное владение
Reference Count & Atomic Reference Count (первый быстрее, второй - потокобезопасен)
```rust
// Rust может вывести все эти типы, но для ясности они указаны явно
let s: Rc<String> = Rc::new("shirataki".to_string());
let t: Rc<String> = s.clone();
let u: Rc<String> = s.clone();
// Клонирование значения типа Rc<T> не приводит к копированию T, вместо этого на него создается еще один указатель, и счетчик ссылок увеличивается на 1
// Все обычные методы String можно вызывать и для Rc<String>, но оно неизменяемо
```

## Ссылки
```rust
// Мапа
use std::collections::HashMap;
type Table = HashMap<String, Vec<String>>;

// После передачи структуры в функцию она будет польностью "потреблена"
fn show(table: Table) {
  for (artist, works) in table {
    println!("работы {}:", artist);
    for work in works {
      println!("  {}", work);
    }
  }
}
```

* Разделяемая ссылка (&e) - сколько угодно, копируемый тип
* Изменяемая ссылки (&mut e) - только одна, некопируемый тип
Пока существуют разделяемые ссылки на значение, даже его владелец не вправе изменить его – значение заблокировано. Никто не сможет модифицировать таблицу table, пока с ней работает функция show.
Аналогично, если существует изменяемая ссылка на значение, то она обладает исключительным доступом к этому значению; с владельцем нельзя производить никаких действий, пока изменяемая ссылка не пропадет.

### в Rust для создания и разыменования ссылки служат операторы & и *, а единственным исключением является оператор ., который осуществляет заимствование и разыменование неявно.

```rust
// Просто хороший пример реализации метода возвращающего Option
struct StringTable {
  elements: Vec<String>,
}

impl StringTable {
  fn find_by_prefix(&self, prefix: &str) -> Option<&String> {
    for i in 0 .. self.elements.len() {
      if self.elements[i].starts_with(prefix) {
        return Some(&self.elements[i]);
       }
    }
    None
  }
}
```

## Match
```rust
match code {
  0 => println!("OK"),
  1 => println!("Перепутались провода"),
  2 => println!("Пользователь спит"),
  _  => println!("Неизвестная ошибка{}", code)
}

match params.get("name") {
  Some(name) => println!("Привет,{}!", name),
  None => println!("Приветствую,незнакомец.")
}


```

## if let
Существует еще одна форма выражения if– iflet:
```rust
// Если выражение expr соответствует образцу pattern, то выполняется блок block1, иначе блок block2
if let pattern = expr {
  block1
} else {
  block2
}

if let Some(cookie) = request.session_cookie {
  return restore_session(cookie);
}

if let Err(err) = present_cheesy_anti_robot_task() {
  log_robot_attempt(err);
  politely_accuse_user_of_being_a_robot();
} else {
  session.mark_as_human();
}

// if let – всего лишь сокращенная запись match с единственным образцом:
match expr {
  pattern=> { block1 }
  _ => { block2}
}
```

## ?
```rust
let output = File::create(filename)?;

//Эквивалентно:
let output = match File::create(filename) {
  Ok(f) => f, // просто output = f
  Err(err) => return Err(err) // Если попадем сюда - произойдет возврат из текущией функции с возвратом ошибки
};
```

## Приведение типов
```rust
let x = 17;              //x имеет тип i32
let index = x as usize;  //преобразовать в типusize
```

## Замыкания
```rust
let is_even = |x| x % 2 == 0;

let is_even = |x: u64| -> bool { x % 2 == 0 };
assert_eq!(is_even(14), true);
```

## Тип Result
```rust
fn get_weather(location: LatLng) -> Result<WeatherReport, io::Error>

match get_weather(hometown) {
  Ok(report) => {
    display_weather(hometown, &report);
  }
  Err(err) => {
    println!("ошибка при запросе погоды:{}", err);
    schedule_weather_retry();
  }
}
```

### Всякие удобные методы:
* result.is_ok() и result.is_err() - bool значение
* result.ok()  возвращает  успешный  результат,  если  таковой  присутствует, в виде значения типа Option<T>
* result.err() возвращает ошибочный результат, если таковой присутствует, в виде значения типа Option<E>
* result.unwrap_or(fallback) возвращает успешный результат, если таковой содержится в result. В противном случае возвращается fallback
  ```rust
  // Сравнительно безопасный прогноз погоды для Южной Калифорнии.
  const THE_USUAL: WeatherReport = WeatherReport::Sunny(72);
  // Получить истинный прогноз погоды, если возможно.
  // Если нет, считать, что погода обычная.
  let report = get_weather(los_angeles).unwrap_or(THE_USUAL);
  display_weather(los_angeles, &report);
  ```
* result.unwrap_or_else(fallback_fn) – то же самое, но вместо возврата подмен-ного значения непосредственно вызывает переданную функцию или замыкание
    ```rust
    let report = get_weather(hometown)
    .unwrap_or_else(|_err| vague_prediction(hometown));
    ```
* result.unwrap()  также  возвращает  успешный  результат,  если  он  содержится в result. Если же result содержит ошибочный результат, то этот метод паникует.
* result.expect(message) – то же, что .unwrap(), но позволяет задать сообщение, которое печатается в случае паники
* result.as_ref() преобразует Result<T,E> в Result<&T,&E>, заимствуя ссылку на успешный или ошибочный результат, содержащийся в имеющемся значении result.
* result.as_mut() – то же самое, но заимствуется изменяемая ссылка. Тип возвращаемого значения – Result<&mutT,&mutE>. Иногда желательно получить доступ к данным внутри result, не унич-тожая их, именно для этого и предназначены методы .as_ref() и .as_mut()

## Псевдонимы типа Result
```rust
// Это означает, что используется псевдоним типа Result
fn remove_file(path: &Path) -> Result<()>

// Здесь  определяется  открытый  тип std::io::Result<T>.  Это  псевдоним  типа Result<T,E>, но в качестве типа ошибки зашит тип std::io::Error
pub type Result<T> = result::Result<T, Error>;
```

## Распространение ошибок
```rust
let weather = get_weather(hometown)?;
```
Поведение ? зависит от того, возвращает ли функция успешный или ошибочный результат.
* В случае успеха оператор разворачивает Result методом unwrap, чтобы получить находящееся внутри него значение. В данном случае типом переменной weather будет не Result<WeatherReport,io::Error>, а просто Weather-Report.
* В случае ошибки оператор сразу же возвращает управление из объемлющей функции, передавая ошибочный результат вверх по цепочке вызовов. Но чтобы этот механизм работал, оператор ? следует использовать только в функциях, возвращающих значение типа Result

? аналогичен следющему коду:
```rust
let weather = match get_weather(hometown) {
  Ok(success_value) => success_value,
  Err(err) => return Err(err)
};
```

### Пример обработки ошибок
```rust
// Обьявляем "универсальный" тип, переваривающий любые ошибки
type GenError = Box<std::error::Error>;
type GenResult<T> = Result<T, GenError>;

use std::io::{self, BufRead};
/// Читать целые числа из текстового файла.
/// Вкаждой строке файла должно быть одно число.
fn read_numbers(file: &mut BufRead) -> GenResult<Vec<i64>> {
  let mut numbers = vec![];
  for line_result in file.lines() {
    let line = line_result?;      // чтение строки может завершиться ошибкой
    numbers.push(line.parse()?);  // разбор целого числа может быть неудачным
  }
  Ok(numbers)
}
```

Для обработки ошибок в main() проще всего использовать метод .expect().
```rust
fn main() {
  calculate_tides().expect("error");  // здесь распространение прекращается
}
```
Если  функция calculate_tides()  возвращает  ошибочный  результат,  то  метод .expect() паникует. Паника в главном потоке влечет за собой печать сообщения об ошибке и завершение программы с ненулевым кодом. В общем-то, такое поведение можно назвать желательным.

Подробный вывод ошибки (всего дерева cause):
```rust
fn main() {
  if let Err(err) = calculate_tides() {
    print_error(&err);
    std::process::exit(1);
  }
}

use std::error::Error;
use std::io::{Write, stderr};

/// Вывести сообщение об ошибке в`stderr`.
///
/// Если  при построении сообщения об ошибке или его выводе
/// произойдет еще одна ошибка, она будет проигнорирована.
fn print_error(mut err: &Error) {
  let _ = writeln!(stderr(), "error: {}", err);
  while let Some(cause) = err.cause() {
    let _ = writeln!(stderr(), "caused by: {}", cause);
    err = cause;
  }
}
```

### Объявление пользовательского типа ошибки
/json/src/error.rs
```rust
#[derive(Debug, Clone)]
pub struct JsonError {
  pub message: String,
  pub line: usize,
  pub column: usize,
}
```
Эта структура будет именоваться json::error::JsonError, и если мы захотим вер-нуть ошибку такого типа, то напишем:
```rust
return Err(JsonError {
  message: "expected ']' at end of array".to_string(),
  line: current_line,
  column: current_column
});
```
Это работает. Но если мы захотим, чтобы наш тип ошибки вел себя как стандартные, то:
```rust
use std;
use std::fmt;
// Ошибки должны допускать распечатку.
impl fmt::Display for JsonError {
  fn fmt(&self, f: &mut fmt::Formatter) -> Result<(), fmt::Error> {
    write!(f, "{} ({}:{})", self.message, self.line, self.column)
  }
}
// Ошибки должны реализовывать характеристику std::error::Error.
impl std::error::Error for JsonError {
  fn description(&self) -> &str {
    &self.message
  }
}
```

## Tests
```rust
/// Этот тест проходит,т олько еслид еление на нуль вызывает панику,
#[test]
#[should_panic(expected="divide by zero")]
fn test_divide_by_zero_error() {
  1 / 0;  //should panic !
}
```


