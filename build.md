# Build Zen

```bash
git clone https://github.com/gorules/zen.git
cd zen
git checkout experiments/ffi

# git has a limit of 100mb for commits

## Workaround 1 - does not work
# https://blog.rust-lang.org/2022/02/24/Rust-1.59.0.html#creating-stripped-binaries
# https://rustrepo.com/repo/johnthagen-min-sized-rust
echo "add to toplevel of Cargo.toml"

echo 'cargo-features = ["strip"]'
echo ''
echo '[profile.release]'
echo 'strip = true'

## Workaround 2 - does not work
# strip does not work :(
# https://github.com/rust-lang/cargo/issues/3483
RUSTFLAGS='-C link-arg=-s' cargo build --release

## Workaround 3 - hammer fist... - does not work
rm -rf target
cargo build --release
cd ./target/release/
rm -rf tmp
mkdir tmp
cd tmp
cp ../libzen_ffi.a .
/usr/bin/ar x *.a
rm *.a
strip *.o
/usr/bin/ar cr libzen_ffi.a *.o
cd ..
mv tmp/libzen_ffi.a .
rm -rf tmp
cd ..
cd ..

# https://github.com/rust-lang/rust/issues/24764#issuecomment-95871256
# https://internals.rust-lang.org/t/rust-staticlibs-and-optimizing-for-size/5746
# https://darkcoding.net/software/a-very-small-rust-binary-indeed/
# https://users.rust-lang.org/t/how-to-use-gc-sections-link-option-to-reduce-target-binary-size/71374
rm -rf target
#RUSTFLAGS="--emit=obj -O -Ctarget-cpu=native -Clink-args=-nostartfiles -Clink-args=-Wl,-n,-N,--no-dynamic-linker" cargo build --release
RUSTFLAGS="-Copt-level=3 --emit=obj -Clink-args=-Wl,--gc-section" cargo build --release
ls -la ./target/release/libzen_ffi.a
ls -la ./target/release/libzen_ffi.so

cp ./target/release/libzen_ffi.a  ../zen-go/deps/linux_amd64
cp  ./target/release/libzen.so  ../zen-go/deps/linux_amd64
cp ./target/release/libzen_ffi.so  ../zen-go/deps/linux_amd64
```
