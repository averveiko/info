```sh
# порождает  оптимизированную  выпускную  сборку.
cargo build --release

# прогоняет все имеющиеся в проекте тесты
cargo test 

# строит проект с зависимостями.
cargo build --verbose
```

## Модули
```rust
// Модуль, размещенный внутри другого исходного кода
mod spores {
  use  cells::Cell;

pub  struct  Spore { 
  ...
}

// pub - видны "извне": let s = spores::produce_spore(&mut factory);
pub  fn  produce_spore(factory: &mut Sporangium) -> Spore {
  ...
}

fn  recombine(parent: &mut Cell) {
  ...
}
  ...
}

// Модуль, размещенный во внешнем файле spores.rs
pub struct Spore {...}
pub fn produce_spore(factory: &mut Sporangium) -> Spore {...}
fn recombine(parent: &mut Cell) {...}
```

## Создание иерархии модулей
```rust
fern_sim/
├──Cargo.toml
└──src/
  ├──main.rs
  ├──spores.rs
  └──plant_structures/
    ├──mod.rs
    ├──leaves.rs
    ├──roots.rs
    └──stems.rs

// В файле main.rs объявляется модуль plant_structures:
  pub mod plant_structures;
// Тем самым мы заставляем Rust загрузить файл plant_structures/mod.rs, в котором объявлены все три подмодуля:
// в plant_structures/mod.rs:
pub mod roots;
pub mod stems;
pub mod leaves

// Импортирование из модуля:
use std::collections::{HashMap, HashSet};  // импортируются оба имени
use std::io::prelude::*;  // импортируется всё
```

```rust
// Псевдонимы типов
type Table = HashMap<String, Vec<String>>;
```
