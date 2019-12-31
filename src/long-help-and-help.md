### `long_help` and `--help`

A message passed to [`App::long_about`] or [`Arg::long_help`] will be displayed whenever
your program is called with `--help` instead of `-h`. Of course, you can
use them via raw methods as described [above](#help-messages).

The more convenient way is to use a so-called "long" doc comment:

```
# use structopt::StructOpt;
#[derive(StructOpt)]
/// Hi there, I'm Robo!
///
/// I like beeping, stumbling, eating your electricity,
/// and making records of you singing in a shower.
/// Pay up, or I'll upload it to youtube!
struct Robo {
    /// Call my brother SkyNet.
    ///
    /// I am artificial superintelligence. I won't rest
    /// until I'll have destroyed humanity. Enjoy your
    /// pathetic existence, you mere mortals.
    #[structopt(long)]
    kill_all_humans: bool
}
```

A long doc comment consists of three parts:
* Short summary
* A blank line (whitespace only)
* Detailed description, all the rest

In other words, "long" doc comment consists of two or more paragraphs,
with the first being a summary and the rest being the detailed description.

**A long comment will result in two method calls**, `help(<summary>)` and
`long_help(<whole comment>)`, so clap will display the summary with `-h`
and the whole help message on `--help` (see below).

So, the example above will be turned into this (details omitted):
```
clap::App::new("<name>")
    .about("Hi there, I'm Robo!")
    .long_about("Hi there, I'm Robo!\n\n\
                 I like beeping, stumbling, eating your electricity,\
                 and making records of you singing in a shower.\
                 Pay up or I'll upload it to youtube!")
// args...
# ;
```

[`App::about`]:      https://docs.rs/clap/2/clap/struct.App.html#method.about
[`App::long_about`]: https://docs.rs/clap/2/clap/struct.App.html#method.long_about
[`Arg::help`]:       https://docs.rs/clap/2/clap/struct.Arg.html#method.help
[`Arg::long_help`]:  https://docs.rs/clap/2/clap/struct.Arg.html#method.long_help