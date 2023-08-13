# Build Zen

```bash
git clone https://github.com/gorules/zen.git
cd zen
git checkout experiments/ffi

# git has a limit of 100mb for commits
cargo build --release
strip ./target/release/libzen_ffi.a
cp ./target/release/libzen_ffi.a  ../zen-go/deps/linux_amd64
```
