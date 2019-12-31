## Help messages

In clap, help messages for the whole binary can be specified
via [`App::about`] and [`App::long_about`] while help messages
for individual arguments can be specified via [`Arg::help`] and [`Arg::long_help`]".

`long_*` variants are used when user calls the program with
`--help` and "short" variants are used with `-h` flag. In `structopt`,
you can use them via [raw methods](#raw-methods), for example:

```
# use structopt::StructOpt;

#[derive(StructOpt)]
#[structopt(about = "I am a program and I work, just pass `-h`")]
struct Foo {
  #[structopt(short, help = "Pass `-h` and you'll see me!")]
  bar: String
}
```

For convenience, doc comments can be used instead of raw methods
(this example works exactly like the one above):

```
# use structopt::StructOpt;

#[derive(StructOpt)]
/// I am a program and I work, just pass `-h`
struct Foo {
  /// Pass `-h` and you'll see me!
  bar: String
}
```

Doc comments on [top-level](#magical-methods) will be turned into
`App::about/long_about` call (see below), doc comments on field-level are
`Arg::help/long_help` calls.

**Important:**
_________________

Raw methods have priority over doc comments!

**Top level doc comments always generate `App::about/long_about` calls!**
If you really want to use the `App::help/long_help` methods (you likely don't),
use a raw method to override the `App::about` call generated from the doc comment.
__________________

[`App::about`]:      https://docs.rs/clap/2/clap/struct.App.html#method.about
[`App::long_about`]: https://docs.rs/clap/2/clap/struct.App.html#method.long_about
[`Arg::help`]:       https://docs.rs/clap/2/clap/struct.Arg.html#method.help
[`Arg::long_help`]:  https://docs.rs/clap/2/clap/struct.Arg.html#method.long_help