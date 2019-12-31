## Subcommands

Some applications, especially large ones, split their functionality
through the use of "subcommands". Each of these act somewhat like a separate
command, but is part of the larger group.
One example is `git`, which has subcommands such as `add`, `commit`,
and `clone`, to mention just a few.

`clap` has this functionality, and `structopt` supports it through enums:

```
# use structopt::StructOpt;

# use std::path::PathBuf;
#[derive(StructOpt)]
#[structopt(about = "the stupid content tracker")]
enum Git {
    Add {
        #[structopt(short)]
        interactive: bool,
        #[structopt(short)]
        patch: bool,
        #[structopt(parse(from_os_str))]
        files: Vec<PathBuf>
    },
    Fetch {
        #[structopt(long)]
        dry_run: bool,
        #[structopt(long)]
        all: bool,
        repository: Option<String>
    },
    Commit {
        #[structopt(short)]
        message: Option<String>,
        #[structopt(short)]
        all: bool
    }
}
# fn main() {}
```

Using `derive(StructOpt)` on an enum instead of a struct will produce
a `clap::App` that only takes subcommands. So `git add`, `git fetch`,
and `git commit` would be commands allowed for the above example.

`structopt` also provides support for applications where certain flags
need to apply to all subcommands, as well as nested subcommands:

```
# use structopt::StructOpt;
# fn main() {}
#[derive(StructOpt)]
struct MakeCookie {
    #[structopt(name = "supervisor", default_value = "Puck", long = "supervisor")]
    supervising_faerie: String,
    /// The faerie tree this cookie is being made in.
    tree: Option<String>,
    #[structopt(subcommand)]  // Note that we mark a field as a subcommand
    cmd: Command
}

#[derive(StructOpt)]
enum Command {
    /// Pound acorns into flour for cookie dough.
    Pound {
        acorns: u32
    },
    /// Add magical sparkles -- the secret ingredient!
    Sparkle {
        #[structopt(short, parse(from_occurrences))]
        magicality: u64,
        #[structopt(short)]
        color: String
    },
    Finish(Finish),
}

// Subcommand can also be externalized by using a 1-uple enum variant
#[derive(StructOpt)]
struct Finish {
    #[structopt(short)]
    time: u32,
    #[structopt(subcommand)]  // Note that we mark a field as a subcommand
    finish_type: FinishType
}

// subsubcommand!
#[derive(StructOpt)]
enum FinishType {
    Glaze {
        applications: u32
    },
    Powder {
        flavor: String,
        dips: u32
    }
}
```

Marking a field with `structopt(subcommand)` will add the subcommands of the
designated enum to the current `clap::App`. The designated enum *must* also
be derived `StructOpt`. So the above example would take the following
commands:

+ `make-cookie pound 50`
+ `make-cookie sparkle -mmm --color "green"`
+ `make-cookie finish 130 glaze 3`
