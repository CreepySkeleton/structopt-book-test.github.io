# Default values


In clap, default values for options can be specified via [`Arg::default_value`].

Of course, you can use as a raw method:
```
# use structopt::StructOpt;
#[derive(StructOpt)]
struct Opt {
    #[structopt(default_value = "", long)]
    prefix: String
}
```

This is quite mundane and error-prone to type the `"..."` default by yourself,
especially when the Rust ecosystem uses the [`Default`] trait for that.
It would be wonderful to have `structopt` to take the `Default_default` and fill it
for you. And yes, `structopt` can do that.

Unfortunately, `default_value` takes `&str` but `Default::default`
gives us some `Self` value. We need to map `Self` to `&str` somehow.

`structopt` solves this problem via [`ToString`] trait.

To be able to use auto-default the type must implement *both* `Default` and `ToString`:

```
# use structopt::StructOpt;
#[derive(StructOpt)]
struct Opt {
    // just leave the `= "..."` part and structopt will figure it for you
    #[structopt(default_value, long)]
    prefix: String // `String` implements both `Default` and `ToString`
}
```

[`Default`]: https://doc.rust-lang.org/std/default/trait.Default.html
[`ToString`]: https://doc.rust-lang.org/std/string/trait.ToString.html
[`Arg::default_value`]: https://docs.rs/clap/2.33.0/clap/struct.Arg.html#method.default_value

