# Specifying argument types

There are three types of arguments that can be supplied to each
(sub-)command:

 - short (e.g. `-h`),
 - long (e.g. `--help`)
 - and positional.

Like clap, structopt defaults to creating positional arguments.

If you want to generate a long argument you can specify either
`long = $NAME`, or just `long` to get a long flag generated using
the field name.  The generated casing style can be modified using
the `rename_all` attribute. See the `rename_all` example for more.

For short arguments, `short` will use the first letter of the
field name by default, but just like the long option it's also
possible to use a custom letter through `short = $LETTER`.

If an argument is renamed using `name = $NAME` any following call to
`short` or `long` will use the new name.

**Attention**: If these arguments are used without an explicit name
the resulting flag is going to be renamed using `kebab-case` if the
`rename_all` attribute was not specified previously. The same is true
for subcommands with implicit naming through the related data structure.

```
use structopt::StructOpt;

#[derive(StructOpt)]
#[structopt(rename_all = "kebab-case")]
struct Opt {
    /// This option can be specified with something like `--foo-option
    /// value` or `--foo-option=value`
    #[structopt(long)]
    foo_option: String,

    /// This option can be specified with something like `-b value` (but
    /// not `--bar-option value`).
    #[structopt(short)]
    bar_option: String,

    /// This option can be specified either `--baz value` or `-z value`.
    #[structopt(short = "z", long = "baz")]
    baz_option: String,

    /// This option can be specified either by `--custom value` or
    /// `-c value`.
    #[structopt(name = "custom", long, short)]
    custom_option: String,

    /// This option is positional, meaning it is the first unadorned string
    /// you provide (multiple others could follow).
    my_positional: String,

    /// This option is skipped and will be filled with the default value
    /// for its type (in this case 0).
    #[structopt(skip)]
    skipped: u32,

}

# fn main() {
# Opt::from_clap(&Opt::clap().get_matches_from(
#    &["test", "--foo-option", "", "-b", "", "--baz", "", "--custom", "", "positional"]));
# }
```
