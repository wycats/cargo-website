---
title: The Manifest Format
---

# The `[package]` Section

The first section in a `Cargo.toml` is `[package]`.

```toml
[package]

name = "hello-world" # the name of the package
version = "1.0.0"    # the current version, obeying semver
authors = [ "wycats@example.com" ]
```

All three of these fields are mandatory. Cargo bakes in the concept of
[Semantic Versioning](http://semver.org/), so make sure you follow some
basic rules:

* Before you reach 1.0, anything goes.
* After 1.0, only make breaking changes when you increment the major
  version. In Rust, breaking changes include adding fields to structs or
  variants to enums. Don't break the build.
* After 1.0, don't add any new public API (no new `pub` anything) in
  tiny versions. Always increment the minor version if you add any new
  `pub` structs, traits, fields, types, functions, methods or anything else.

## The `build` Field (optional)

You can specify a script that Cargo should execute before invoking
`rustc`. You can use this to compile C code that you will [link][1] into
your Rust code, for example.

[1]: http://doc.rust-lang.org/rust.html#external-blocks

```toml
[package]

# ...

build = "make"
```

# The `[dependencies.*]` Sections

You list dependencies using `[dependencies.<name>]`. For example, if you
wanted to depend on both `hammer` and `color`:

```toml
[package]

# ...

[dependencies.hammer]

version = "0.5.0" # optional
git = "https://github.com/wycats/hammer.rs"

[dependencies.color]

git = "https://github.com/bjz/color-rs"
```

You can specify the source of a dependency in one of two ways at the moment:

* `git = "<git-url>"`: A git repository with a `Cargo.toml` in its root. The
  `rev`, `tag`, and `branch` options are also recognized to use something other
  than the `master` branch.
* `path = "<relative-path>"`: A path relative to the current `Cargo.toml`
  with a `Cargo.toml` in its root.

Soon, you will be able to load packages from the Cargo registry as well.

# The `[[lib]]` and `[[bin]]` Sections

A Cargo package can export at most one library, and can build as many
executables as you like.

The double-brackets around `lib` and `bin` indicate that the sections
may be repeated to represent a list. At the moment, Cargo only supports
a single `lib`, but we use the double-bracket syntax to reserve the
ability to support multiple `lib` sections in the future.

# The `lib` Section

If your project produces a library (the main `.rs` file does not have a
`main` function), you should include a `[[lib]]` section.

```toml
[package]

# ...

[[lib]]

name = "color" # the name of the library, the same as the package name
```

## The `crate-type` Field

Top-level packages can specify the kind of library for Cargo to build:

```toml
[package]

# ...

[[lib]]

name = "hello-world"
crate-type = ["dylib", "staticlib"]
```

This will produce a `.a` file and an `.so` (or `.dylib` on OSX).

## The `path` Field

You can specify the location of the main `.rs` file by using the `path`
field.

```toml
[package]

# ...

[[lib]]

name = "hello-world"
path = "src/lib.rs"
```

As a temporary measure, if you use the `path` field, you **must**
specify a `#[crate_id=<name>]` inside the `.rs` file. Rust will soon
support passing this information from the command-line to `rustc`, which
will change this requirement.

# The `bin` Section

Cargo will produce one executable for every `bin` section.

If you don't specify a `path`, the path defaults to:

* If you don't also have a `lib`, `src/<name>.rs`
* If you also have a `lib`, `src/bin/<name>.rs`

As a temporary measure, if you use the `path` field, and the crate name
is not **exactly** the same as the last part of the file name, you
**must** specify a `#[crate_id=<name>]` inside the `.rs` file. Rust will
soon support passing this information from the command-line to `rustc`,
which will change this requirement.

```toml
[package]

# ...

[[bin]]

name = "hello-world"

[[bin]]

name = "hiya" # defaults to src/fun-times.rs

[[bin]]

name = "folks"
# you must specify a #[crate_id=folks] inside this file
path = "src/exe/folkster.rs"
```
