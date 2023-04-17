# gm-rs

A Pure Rust High-Performance Implementation of China's Standards of Encryption Algorithms ZUC

## Usage

Add this to your `Cargo.toml`:

```toml
[dependencies]
gm-zuc = "0.9.0"
```

## Example

```rust
use crate::ZUC;

fn main() {
    let key = [0u8; 16];
    let iv = [0u8; 16];
    let mut zuc = ZUC::new(&key, &iv);
    let rs = zuc.generate_keystream(2);
    for z in rs {
        println!("{:x}", z)
    }
}

```