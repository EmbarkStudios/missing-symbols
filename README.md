```rust
#[no_mangle]
#[used]
pub static FOO: u64 = 4242;
```

```
cargo build --release --target wasm32-unknown-unknown && wasm2wat target/wasm32-unknown-unknown/release/missing_symbol.wasm > symbol.wast
```

```
(module
  (table (;0;) 1 1 funcref)
  (memory (;0;) 16)
  (global (;0;) (mut i32) (i32.const 1048576))
  (global (;1;) i32 (i32.const 1048576))
  (global (;2;) i32 (i32.const 1048576))
  (export "memory" (memory 0))
  (export "__data_end" (global 1))
  (export "__heap_base" (global 2)))
```

```
RUSTFLAGS="-C link-dead-code" cargo build --release --target wasm32-unknown-unknown && wasm2wat target/wasm32-unknown-unknown/release/missing_symbol.wasm > symbol.wast
```

```
(module
  (table (;0;) 1 1 funcref)
  (memory (;0;) 17)
  (global (;0;) (mut i32) (i32.const 1048576))
  (global (;1;) i32 (i32.const 1048584))
  (global (;2;) i32 (i32.const 1048584))
  (global (;3;) i32 (i32.const 1048576))
  (export "memory" (memory 0))
  (export "__data_end" (global 1))
  (export "__heap_base" (global 2))
  (export "FOO" (global 3))
  (data (;0;) (i32.const 1048576) "\92\10\00\00\00\00\00\00"))
```
