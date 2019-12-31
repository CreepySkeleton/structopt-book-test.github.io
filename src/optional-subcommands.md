### Optional subcommands

Subcommands may be optional:

```
# use structopt::StructOpt;
# fn main() {}
#[derive(StructOpt)]
struct Foo {
    file: String,
    #[structopt(subcommand)]
    cmd: Option<Command>
}

#[derive(StructOpt)]
enum Command {
    Bar,
    Baz,
    Quux
}
```
