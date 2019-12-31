# Attributes

`#[structopt(...)]` attributes fall into two categories:

- `structopt`'s own [magical methods](#magical-methods).

   They are used by `structopt` itself. They come mostly in
   `attr = ["whatever"]` form, but some `attr(args...)` also exist.

- [`raw` attributes](#raw-methods).

    They represent explicit `clap::Arg/App` method calls.
    They are what used to be explicit `#[structopt(raw(...))]` attrs in pre-0.3 `structopt`

Every `#[structopt(...)]` attribute looks like comma-separated sequence of **methods**:
```rust,ignore
#[structopt(
    short, // method with no arguments - always magical
    long = "--long-option", // method with one argument
    required_if("out", "file"), // method with one and more args
    parse(from_os_str = path::to::parser) // some magical methods have their own syntax
)]
```

`#[structopt(...)]` attributes can be placed on top of `struct`, `enum`,
`struct` field or `enum` variant. Attributes on top of `struct` or `enum`
represent `clap::App` method calls, field or variant attributes correspond
to `clap::Arg` method calls.

In other words, the `Opt` struct from the example above
will be turned into this (*details omitted*):

```rust
# use structopt::clap::{Arg, App};
App::new("example")
    .version("0.2.0")
    .about("An example of StructOpt usage.")
.arg(Arg::with_name("debug")
    .help("Activate debug mode")
    .short("debug")
    .long("debug"))
.arg(Arg::with_name("speed")
    .help("Set speed")
    .short("v")
    .long("velocity")
    .default_value("42"))
// and so on
# ;
```
