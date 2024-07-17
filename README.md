# lazyinit

[![Crates.io](https://img.shields.io/crates/v/lazyinit)](https://crates.io/crates/lazyinit)
[![Docs.rs](https://docs.rs/lazyinit/badge.svg)](https://docs.rs/lazyinit)
[![CI](https://github.com/arceos-org/lazyinit/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/arceos-org/lazyinit/actions/workflows/ci.yml)

Initialize a static value lazily.

Unlike [`lazy_static`][1], this crate does not provide concurrency safety.
The value **MUST** be used after only **ONE** initialization. However, it
can be more efficient, as there is no need to check whether other threads
are also performing initialization at the same time.

## Examples

```rust
use lazyinit::LazyInit;

static VALUE: LazyInit<u32> = LazyInit::new();
assert!(!VALUE.is_init());
// println!("{}", *VALUE); // panic: use uninitialized value
assert_eq!(VALUE.try_get(), None);

VALUE.init_by(233);
// VALUE.init_by(666); // panic: already initialized
assert!(VALUE.is_init());
assert_eq!(*VALUE, 233);
assert_eq!(VALUE.try_get(), Some(&233));
```

[1]: https://docs.rs/lazy_static/latest/lazy_static/
