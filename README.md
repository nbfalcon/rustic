# Rustic

[![MELPA](https://melpa.org/packages/rustic-badge.svg)](https://melpa.org/#/rustic)
[![Build Status](https://travis-ci.com/brotzeit/rustic.svg?branch=master)](https://travis-ci.com/brotzeit/rustic)

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [Intro](#intro)
- [Installation](#installation)
- [Rustfmt](#rustfmt)
- [Rust Language Server](#rust-language-server)
- [Clippy](#clippy)
- [Org-babel](#org-babel)
- [Popup](#popup)
- [Cargo outdated](#cargo-outdated)
- [Contributing](#contributing)

<!-- markdown-toc end -->

# Intro

This package is a fork of [rust-mode](https://github.com/rust-lang/rust-mode)

Differences with rust-mode:

- cargo popup
- multiline error parsing
- translation of ANSI control sequences through [xterm-color](https://github.com/atomontage/xterm-color)
- async org babel
- custom compilation process
- rustfmt errors in a rust compilation mode
- automatic RLS configuration with [eglot](https://github.com/joaotavora/eglot) or [lsp-mode](https://github.com/emacs-lsp/lsp-mode)
- cask for testing
- requires emacs 26
- etc.

# Installation

Simply put `(use-package rustic)` in your config and most stuff gets configured automatically. 

If you have `rust-mode` installed, ensure it is required before rustic since it has to be removed
from `auto-mode-alist`. However you only need `rust-mode` if you want to use `emacs-racer`. There's some stuff that isn't included in rustic.

# Rustfmt

You can format your code with `rustic-format-buffer` or `rustic-cargo-fmt`.
The variable `rustic-format-on-save` allows you to turn off auto-format on save.
Rustic uses the function `rustic-save-some-buffers` for saving buffers before compilation. 
If you want buffers to be saved automatically, you can change the value of `buffer-save-without-query`.

Note: Rust edition 2018 requires a `rustfmt.toml` file.

# Rust Language Server

The default package is `lsp-mode`. But you can also use `eglot` or no RLS client with `nil`.

``` emacs-lisp
(setq rustic-rls-pkg 'eglot)
```

# Clippy

Rustic automatically configures a checker that runs clippy when `flycheck` is required.
In case you use `flymake`, you have to take care of the configuration yourself.

Currently cargo does not display the correct installation command for some toolchains when
clippy isn't installed. 
If you have problems try it with `rustup component add --toolchain nightly clippy`.

Use `rustic-cargo-clippy` to view the results in a derived compilation mode.

# Org-babel

The executed blocks run asynchronously and a running babel process is indicated by a spinner in
the mode line. It can be turned off with `rustic-display-spinner`.

If you prefer to see the output in a seperate buffer you can set `rustic-babel-display-compilation-buffer` to `t`.

It's also possible to use crates in rust babel blocks:

```
#+BEGIN_SRC rustic :crates '(("regex" . "0.2") ("darling" . "0.1"))
fn main() {
    println!("{}", "foo");
}
#+END_SRC
```

# Popup

You can execute commands with `rustic-popup`. The list of commands can be customized
with `rustic-popup-commands`. It's also possible to view the command's flags with `h`.
The command `rustic-popup-default-action` (`RET` or `TAB`) allows you to change:

- `RUST_BACKTRACE` environment variable
- `compilation-arguments` for `recompile`
- arguments for `cargo test`

![](https://raw.githubusercontent.com/brotzeit/rustic/master/img/popup.png)

# Cargo outdated

Use `rustic-cargo-outdated` to get a list of dependencies that are out of date. The results 
are displayed in `tabulated-list-mode` and you can use most commands you know from the emacs
package menu.

- `u` mark single crate for upgrade
- `U` mark all upgradable crates
- `m` remove mark
- `x` perform marked package menu actions
- `r` refresh crate list
- `q` quit window

![](https://raw.githubusercontent.com/brotzeit/rustic/master/img/outdated.png)

# Contributing

PRs, feature requests and bug reports are very welcome.
