---
title: Cargo, Rust's Package Manager
---

# Let's Get Started

To start, add a `Cargo.toml` to the root of your project. Here's a
simple one to get you started.

```toml
[package]

name = "hello-world"
version = "0.1.0"
authors = [ "wycats@example.com" ]

[[bin]]

name = "hello-world" # the name of the executable to generate
```

Next, add `src/hello-world.rs` to your project.

```rs
fn main() {
    println!("Hello world!");
}
```

And compile it:

<pre><code class="highlight"><span class="gp">$</span> cargo build
<span style="font-weight: bold"
class="s1">   Compiling</span> hello-world v0.1.0</code></pre>

```shell
$ ./target/hello-world
Hello world!
```

# Depend on a Library

To depend on a library, add it to your `Cargo.toml`.

```toml
[package]

name = "hello-world"
version = "0.1.0"
authors = [ "wycats@example.com" ]

[[bin]]

name = "hello-world" # the name of the executable to generate

[dependencies.color]

git = "https://github.com/bjz/color-rs.git"
```

You added the `color` library, which provides simple conversions
between different color types.

Now, you can pull in that library using `extern crate` in
`hello-world.rs`.

```rs
extern crate color;

use color::{RGB, ToHSV};

fn main() {
    println!("Converting RGB to HSV!");
    let red = RGB::new(255u8, 0, 0);
    println!("HSL: {}", red.to_hsv::<f32>());
}
```

Compile it:

<pre><code class="highlight"><span class="gp">$</span> cargo build
<span style="font-weight: bold" class="s1">    Updating</span> git repository `https://github.com/bjz/color-rs.git`
<span style="font-weight: bold" class="s1">   Compiling</span> color v1.0.0 (https://github.com/bjz/color-rs.git)
<span style="font-weight: bold" class="s1">   Compiling</span> hello-world v0.1.0</code></pre>

```shell
$ ./target/hello-world
Converting RGB to HSV!
HSV: HSV { h: 0, s: 1, v: 1 }
```

# Recompiling

If you modify `hello-world.rs` and recompile, Cargo will intelligently
skip recompiling `color`, which is still fresh.

<pre><code class="highlight"><span class="gp">$</span> touch src/hello-world.rs
<span class="gp">$</span> cargo build
<span style="font-weight: bold" class="s1">       Fresh</span> color v1.0.0 (https://github.com/bjz/color-rs.git)
<span style="font-weight: bold" class="s1">   Compiling</span> hello-world v0.1.0</code></pre>

To update the repository from Github, pass the `--update-remotes` (or
`-u`) flag to `cargo build`.


<pre><code class="highlight"><span class="gp">$</span> touch src/hello-world.rs
<span class="gp">$</span> cargo build -u
<span style="font-weight: bold" class="s1">    Updating</span> git repository `https://github.com/bjz/color-rs.git`
<span style="font-weight: bold" class="s1">   Compiling</span> color v1.0.0 (https://github.com/bjz/color-rs.git)
<span style="font-weight: bold" class="s1">   Compiling</span> hello-world v0.1.0</code></pre>
