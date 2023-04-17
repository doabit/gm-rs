# gm-rs

A Pure Rust High-Performance Implementation of China's Standards of Encryption Algorithms SM2

## Usage

Add this to your `Cargo.toml`:

```toml
[dependencies]
gm-sm2 = "0.9.0"
```

## Example

### encrypt & decrypt

```rust
 use crate::key::{gen_keypair, CompressModle};

fn main() {
    let (pk, sk) = gen_keypair(CompressModle::Compressed).unwrap();
    let msg = "你好 world,asjdkajhdjadahkubbhj12893718927391873891,@@！！ world,1231 wo12321321313asdadadahello world，hello world".as_bytes();
    let encrypt = pk.encrypt(msg).unwrap();
    let plain = sk.decrypt(&encrypt).unwrap();
    assert_eq!(msg, plain)
}

```

### sign & verify

```rust
use crate::signature;

fn main() {
    let msg = b"hello";
    let (pk, sk) = gen_keypair(CompressModle::Compressed).unwrap();
    let signature = sk.sign(None, msg).unwrap();
    pk.verify(None, msg, &signature).unwrap()
}

```


### key exchange
```rust
use crate::exchange::Exchange;

fn main() {
    let id_a = "alice123@qq.com";
    let id_b = "bob456@qq.com";

    let (pk_a, sk_a) = gen_keypair(CompressModle::Compressed).unwrap();
    let (pk_b, sk_b) = gen_keypair(CompressModle::Compressed).unwrap();

    let mut user_a = Exchange::new(8, Some(id_a), &pk_a, &sk_a, Some(id_b), &pk_b).unwrap();
    let mut user_b = Exchange::new(8, Some(id_b), &pk_b, &sk_b, Some(id_a), &pk_a).unwrap();

    let ra_point = user_a.exchange_1().unwrap();
    let (rb_point, sb) = user_b.exchange_2(&ra_point).unwrap();
    let sa = user_a.exchange_3(&rb_point, sb).unwrap();
    let succ = user_b.exchange_4(sa, &ra_point).unwrap();
    println!("test_key_exchange = {}", succ);
    assert_eq!(user_a.k, user_b.k);
}

```

## Reference
[libsm](https://github.com/citahub/libsm)