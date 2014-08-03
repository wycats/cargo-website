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

# Going farther

For more details on using Cargo, check out the [Cargo Guide](/guide.html)
