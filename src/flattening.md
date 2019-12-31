## Flattening

It can sometimes be useful to group related arguments in a substruct,
while keeping the command-line interface flat. In these cases you can mark
a field as `flatten` and give it another type that derives `StructOpt`:

```
# use structopt::StructOpt;
# fn main() {}
#[derive(StructOpt)]
struct Cmdline {
    /// switch on verbosity
    #[structopt(short)]
    verbose: bool,
    #[structopt(flatten)]
    daemon_opts: DaemonOpts,
}

#[derive(StructOpt)]
struct DaemonOpts {
    /// daemon user
    #[structopt(short)]
    user: String,
    /// daemon group
    #[structopt(short)]
    group: String,
}
```

In this example, the derived `Cmdline` parser will support the options `-v`,
`-u` and `-g`.

This feature also makes it possible to define a `StructOpt` struct in a
library, parse the corresponding arguments in the main argument parser, and
pass off this struct to a handler provided by that library.
