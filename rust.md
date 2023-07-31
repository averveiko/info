# Rust conspect

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

i32 – 32-разрядное целое со знаком, u8 – 8-разрядное целое без знака

Типы isize и usize соответствуют целым размерам указателя со знаком и без знака: 32 разряда на 32-разрядной платформе и 64 – на 64-разрядной.

f32, f64 aka float и double в C и C++

любой блок, заключенный в фигурные скобки, может выступать в роли выражения:
```rust
{
  x.cos()
}
```

u64::from_str(&arg).expect("error parsing argument") // Распарсить, либо вернуть ошибку

