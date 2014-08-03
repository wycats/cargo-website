---
title: Cargo, Rust's Package Manager
---

# Installing

The easiest way to get Cargo is to get the Rust nightly build by using
the `rustup` script:

```shell
$ curl www.rust-lang.org/rustup.sh | sudo bash
```

This will get you the latest nightly build for your platform along with
the latest Cargo. You can run this every day to get the latest updates.

# Let's Get Started

To start a new project with Cargo, use `cargo new`:

```shell
$ cargo new hello_world --bin
```

We're passing `--bin` because we're making a binary program: if we
were making a library, we'd leave it off.

Let's check out what Cargo has generated for us:

```shell
$ cd hello_world
$ tree .
.
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

This is all we need to get started. First, let's check out `Cargo.toml`:

```toml
[package]

name = "hello_world"
version = "0.0.1"
authors = ["Yehuda Katz <wycats@example.com>"]
```

This is called a **manifest**, and it contains all of the metadata that Cargo
needs to compile your project. 

Here's what's in `src/main.rs`:

```rs
fn main() {
    println!("Hello world!");
}
```

Cargo generated a 'hello world' for us. Let's compile it:

<pre><code class="highlight"><span class="gp">$</span> cargo build
<span style="font-weight: bold"
class="s1">   Compiling</span> hello-world v0.1.0</code></pre>

And then run it:

```shell
$ ./target/hello-world
Hello world!
```

We can also use `cargo run` to compile and then run it, all in one step:

<pre><code class="highlight"><span class="gp">$</span> cargo run
<span style="font-weight: bold"
class="s1">   Fresh</span> hello-world v0.1.0
<span style="font-weight: bold"
class="s1">   Running</span> `target/hello_world`
Hello world!</code></pre>

You'll now notice a new file, `Cargo.lock`. It contains information about our
dependencies. Since we don't have any yet, it's not very interesting. Let's
add a dependency.

# Depend on a Library

To depend on a library, add it to your `Cargo.toml`.

```toml
[package]

name = "hello-world"
version = "0.1.0"
authors = ["Yehuda Katz <wycats@example.com>"]

[dependencies.color]

git = "https://github.com/bjz/color-rs.git"
```

You added the `color` library, which provides simple conversions
between different color types.

Now, you can pull in that library using `extern crate` in
`main.rs`.

```rs
extern crate color;

use color::{RGB, ToHSV};

fn main() {
    println!("Converting RGB to HSV!");
    let red = RGB::new(255u8, 0, 0);
    println!("HSV: {}", red.to_hsv::<f32>());
}
```

Let's tell Cargo to fetch this new dependency and update the `Cargo.lock`:

<pre><code class="highlight"><span class="gp">$</span> cargo update color
<span style="font-weight: bold" class="s1">    Updating</span> git repository `https://github.com/bjz/color-rs.git`</code></pre>

Compile it:

<pre><code class="highlight"><span class="gp">$</span> cargo run
<span style="font-weight: bold" class="s1">   Compiling</span> color v1.0.0 (https://github.com/bjz/color-rs.git#bf739419)
<span style="font-weight: bold" class="s1">   Compiling</span> hello-world v0.1.0
$ ./target/hello-world
Converting RGB to HSV!
HSV: HSV { h: 0, s: 1, v: 1 }</code></pre>

We just specified a `git` repository for our dependency, but our `Cargo.lock`
contains the exact information about which revision we used:

```toml
[root]
name = "hello_world"
version = "0.0.1"
dependencies = [
 "color 0.0.1 (git+https://github.com/bjz/color-rs.git#bf739419e2d31050615c1ba1a395b474269a4)",
]

[[package]]
name = "color"
version = "0.0.1"
source = "git+https://github.com/bjz/color-rs.git#bf739419e2d31050615c1ba1a395b474269a4"
```

Now, if `color-rs` gets updated, we will still build with the same revision, until
we choose to `cargo update` again.

# Recompiling

If you modify `main.rs` and recompile, Cargo will intelligently
skip recompiling `color`, which is still fresh.

<pre><code class="highlight"><span class="gp">$</span> touch src/main.rs
<span class="gp">$</span> cargo build
<span style="font-weight: bold" class="s1">       Fresh</span> color v1.0.0 (https://github.com/bjz/color-rs.git#bf739419)
<span style="font-weight: bold" class="s1">   Compiling</span> hello-world v0.1.0</code></pre>

# Going farther

For more details on using Cargo, check out the [Cargo Guide](/guide.html)
