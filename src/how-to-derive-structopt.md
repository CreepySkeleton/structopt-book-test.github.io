## How to derive(StructOpt)

First, let's look at the example:

```rust,should_panic
use std::path::PathBuf;
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
#[structopt(name = "example", about = "An example of StructOpt usage.")]
struct Opt {
    /// Activate debug mode
    // short and long flags (-d, --debug) will be deduced from the field's name
    #[structopt(short, long)]
    debug: bool,

    /// Set speed
    // we don't want to name it "speed", need to look smart
    #[structopt(short = "v", long = "velocity", default_value = "42")]
    speed: f64,

    /// Input file
    #[structopt(parse(from_os_str))]
    input: PathBuf,

    /// Output file, stdout if not present
    #[structopt(parse(from_os_str))]
    output: Option<PathBuf>,

    /// Where to write the output: to `stdout` or `file`
    #[structopt(short)]
    out_type: String,

    /// File name: only required when `out` is set to `file`
    #[structopt(name = "FILE", required_if("out_type", "file"))]
    file_name: String,
}

fn main() {
    let opt = Opt::from_args();
    println!("{:?}", opt);
}
```

So `derive(StructOpt)` tells Rust to generate a command line parser,
and the various `structopt` attributes are simply
used for additional parameters.

First, define a struct, whatever its name. This structure
corresponds to a `clap::App`, its fields correspond to `clap::Arg`
(unless they're [subcommands](#subcommands)),
and you can adjust these apps and args by `#[structopt(...)]` [attributes](#attributes).

**Note:**
_________________
Keep in mind that `StructOpt` trait is more than just `from_args` method.
It has a number of additional features, including access to underlying
`clap::App` via `StructOpt::clap()`. See the
[trait's reference documentation](trait.StructOpt.html).
_________________
