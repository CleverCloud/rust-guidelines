# Clever Cloud rust guidelines

Clever Cloud uses rust in a few projects, both open and closed-source.
This document serves as a reference to try to maintain consistency across this projects,
as well as helping rust newcomers getting up to speed by listing a few useful crates.

## Disclaimer

This is only recommendation-level stuff, based on our experience, not a blessing nor
an indictment of libraries / patterns.

YMMV, and so on (**especially** the `rustfmt` config).

## Common advice

### Installation

Use [rustup](https://rustup.rs/) to install rust, avoid OS packages (unless you
know exactly what you're doing / you're @keruspe).

#### Tooling

`rustfmt` and `clippy` installation instructions tend to change from time to
time, here's the latest instructions, but please check online to make sure it's
still up-to-date.

```sh
# Install rustfmt
rustup run nightly cargo install rustfmt-nightly

# Install clippy
rustup run nightly cargo install clippy
```

Intellij has good rust support, as well as VSCode, with the
[vscode-rust](https://github.com/editor-rs/vscode-rust) extension.  The
extension will install everything (including RLS, so no need to install it
directly).

### Avoid nightly

Try to stay in `stable` if possible. If you're forced to use `nightly`, please describe
why in the readme. List the unstable features so that you can go back to stable when
possible.

### Keep dependencies fresh

Regularly run `cargo outdated` to update dependencies.

### No warnings, use rustfmt && clippy

See <resources/rustfmt.config> for the common `rustfmt` configuration.

Run `clippy` on your code and handle all the warnings. Disable false positives,
but try to avoid top-level things like `#![allow(dead_code)]`.

### Logging

Use `log` along with `env_logger`, send log to `stdout` / `stderr` unless you have a good reason not to.

### Error management

Never call `unwrap` / `expect` outside of `main` (or init functions only called at startup).
If possible, try to only call `unwrap` / `expect` once in main, and use helper functions returning
`Result` for everything else.

Use `?` to combine results with heterogeneous error types, use `Result<Value, Box<std::error::Error>>`
as exposed return types.

### Proper command-line parsing

Use `structopt-derive` for CLI tools, avoid `clap` if possible as it only provides unstructured data
you still have to parse manually. Avoid directly using `std::Args` directly.

### Tokio

Use the `conservative_impl_trait` feature, which drastically improves compilation errors and should be in stable
_soonish_.

### Use Continuous integration

Rust integrates unit testing, and the ecosystem has a lot of tools to check your code runs properly.
See <resources/.travis.yml> for an example Travis CI configuration.

## Useful crates

### HTTP requests

`reqwest` is quite easy to use. Avoid directly using `hyper` for simple HTTP requests

### RabbitMQ

`lapin` works well and is full-featured.

### DB access

We've got good experience with `diesel` (on a sqlite db).

### Serialization

Prefer serde-based solutions if no definition language is available

#### Protobuf

Use `prost`.

### HTTP servers

TBD
