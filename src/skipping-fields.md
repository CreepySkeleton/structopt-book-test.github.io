## Skipping fields

Sometimes you may want to add a field to your `Opt` struct that is not
a command line option and `clap` should know nothing about it. You can ask
`structopt` to skip the field entirely via `#[structopt(skip = value)]`
(`value` must implement `Into<FieldType>`)
or `#[structopt(skip)]` if you want assign the field with `Default::default()`
(obviously, the field's type must implement `Default`).

```
# use structopt::StructOpt;
#[derive(StructOpt)]
pub struct Opt {
    #[structopt(long, short)]
    number: u32,

    // these fields are to be assigned with Default::default()

    #[structopt(skip)]
    k: String,
    #[structopt(skip)]
    v: Vec<u32>,

    // these fields get set explicitly

    #[structopt(skip = vec![1, 2, 3])]
    k2: Vec<u32>,
    #[structopt(skip = "cake")] // &str implements Into<String>
    v2: String,
}
```
