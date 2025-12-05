# Rust Master Note

**Tags:** #rust #programming #systems #performance #safety
**Status:** 🟡 In Bearbeitung
**Docs:** [The Rust Book](https://doc.rust-lang.org/book/) | [Rust by Example](https://doc.rust-lang.org/rust-by-example/) | [Crates.io](https://crates.io/)

---

## 1. Warum Rust?
Rust bietet **Memory Safety** ohne Garbage Collector. Das bedeutet:
* Extrem schnell (vergleichbar mit C++).
* Sicher vor Speicherfehlern (Null-Pointer, Buffer Overflows).
* Modernes Tooling (Cargo ist Paketmanager und Build-Tool in einem).

## 2. Cargo Workflow (Projektmanagement)
Cargo ist dein bester Freund. Es managed Dependencies, Kompilierung und Tests.

| Befehl | Beschreibung |
| :--- | :--- |
| `cargo new <name>` | Erstellt ein neues Projekt (Ordnerstruktur + git). |
| `cargo check` | **Wichtig:** Prüft nur Syntax/Fehler (viel schneller als Build). |
| `cargo build` | Kompiliert das Projekt (Debug Mode, langsam, große Binary). |
| `cargo run` | Kompiliert und führt die `.exe` sofort aus. |
| `cargo build --release` | Baut für Produktion (Maximale Optimierung, dauert länger). |
| `cargo fmt` | Formatiert deinen Code automatisch (Standard-Style). |
| `cargo clippy` | Ein Linter, der dir Tipps gibt, wie du "besseres" Rust schreibst. |

## 3. Core Konzept: Ownership & Borrowing
Das Herzstück von Rust. Der "Borrow Checker" prüft dies zur Compile-Zeit.

### Die 3 Regeln des Ownership
1.  Jeder Wert hat eine Variable als **Owner**.
2.  Es kann immer nur **einen** Owner gleichzeitig geben.
3.  Wenn der Owner "out of scope" geht (Block endet), wird der Wert gelöscht (Dropped).

### Borrowing (Ausleihen)
Anstatt Ownership zu übergeben, leihen wir Referenzen aus (`&`).

* **Immutable Reference (`&T`):** Beliebig viele erlaubt. Nur Lesen.
* **Mutable Reference (`&mut T`):** Nur **eine** gleichzeitig erlaubt. Lesen & Schreiben.
* **Regel:** Du darfst entweder viele Leser ODER einen Schreiber haben. Nie beides gleichzeitig (verhindert Data Races).

```rust
let s1 = String::from("Hallo");
let len = calculate_length(&s1); // s1 wird nur ausgeliehen (&)
// s1 ist hier noch gültig!

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

## 4. Syntax Cheatsheet

### Variablen & Datentypen

Variablen sind standardmäßig **immutable** (unveränderlich).

```Rust
let x = 5;          // Immutable
let mut y = 10;     // Mutable (veränderbar)
y = 15;

const MAX_POINTS: u32 = 100_000; // Konstanten
```
### Funktionen

Der letzte Ausdruck (ohne Semikolon) ist der **Return Value**.
```rust
fn add(a: i32, b: i32) -> i32 {
    a + b  // Kein 'return' nötig, kein Semikolon!
}
```


```rust
struct User {
    username: String,
    active: bool,
}

impl User {
    fn new(name: String) -> User {
        User { username: name, active: true }
    }
}
```

### Control Flow & Pattern Matching

`match` ist wie `switch` auf Steroiden. Es muss **alle** Fälle abdecken.
```rust
let number = 3;

match number {
    1 => println!("Eins"),
    2 | 3 => println!("Zwei oder Drei"),
    _ => println!("Was anderes"), // Default Case
}
```

## 5. Fehlerbehandlung (Result & Option)

Rust hat **keine Exceptions** und **kein Null**.

### Option `<T>` (Was tun bei "Nichts"?)

Anstatt `null` gibt es `Option::Some(value)` oder `Option::None`.

```rust
let x: Option<i32> = Some(5);
let y: Option<i32> = None;
```

### Result `<T, E>` (Fehler können passieren)

Rückgabetyp für Operationen, die schiefgehen können (z.B. Datei öffnen).

- `Ok(T)`: Erfolg, enthält Wert.
    
- `Err(E)`: Fehler, enthält Fehlermeldung.
```rust
// Der '?' Operator: Wenn Fehler -> return Error, sonst gib Wert.
use std::fs::File;
use std::io;

fn read_file() -> Result<String, io::Error> {
    let mut f = File::open("hello.txt")?; // ? propagiert Fehler
    let mut s = String::new();
    // ...
    Ok(s)
}
```