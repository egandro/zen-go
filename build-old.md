# Build Zen

```bash
git clone https://github.com/gorules/zen.git
cd zen
git checkout experiments/ffi

sed  's/staticlib/cdylib/g' -i bindings/c/Cargo.toml
cargo build --release

# build time
export CGO_LDFLAGS="-Lwhatever/zen/target/release"

# run time
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/whatever/zen/target/release"

# create deb/rpm installer
```
