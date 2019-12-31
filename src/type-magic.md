# Type magic

One of major things that makes `structopt` so awesome is it's type magic.
Do you want optional positional argument? Use `Option<T>`! Or perhaps optional argument
that optionally takes value (`[--opt=[val]]`)? Use `Option<Option<T>>`!

Here is the table of types and `clap` methods they correspond to:

Type                         | Effect                                            | Added method call to `clap::Arg`
-----------------------------|---------------------------------------------------|--------------------------------------
`bool`                       | `true` if the flag is present                     | `.takes_value(false).multiple(false)`
`Option<T: FromStr>`         | optional positional argument or option            | `.takes_value(true).multiple(false)`
`Option<Option<T: FromStr>>` | optional option with optional value               | `.takes_value(true).multiple(false).min_values(0).max_values(1)`
`Vec<T: FromStr>`            | list of options or the other positional arguments | `.takes_value(true).multiple(true)`
`Option<Vec<T: FromStr>`     | optional list of options                          | `.takes_values(true).multiple(true).min_values(0)`
`T: FromStr`                 | required option or positional argument            | `.takes_value(true).multiple(false).required(!has_default)`

The `FromStr` trait is used to convert the argument to the given
type, and the `Arg::validator` method is set to a method using
`to_string()` (`FromStr::Err` must implement `std::fmt::Display`).
If you would like to use a custom string parser other than `FromStr`, see
the [same titled section](#custom-string-parsers) below.

**Important:**
_________________
Pay attention that *only literal occurrence* of this types is special, for example
`Option<T>` is special while `::std::option::Option<T>` is not.

If you need to avoid special casing you can make a `type` alias and
use it in place of the said type.
_________________

**Note:**
_________________
`bool` cannot be used as positional argument unless you provide an explicit parser.
If you need a positional bool, for example to parse `true` or `false`, you must
annotate the field with explicit [`#[structopt(parse(...))]`](#custom-string-parsers).
_________________

Thus, the `speed` argument is generated as:

```
# extern crate clap;
# fn parse_validator<T>(_: String) -> Result<(), String> { unimplemented!() }
# fn main() {
clap::Arg::with_name("speed")
    .takes_value(true)
    .multiple(false)
    .required(false)
    .validator(parse_validator::<f64>)
    .short("v")
    .long("velocity")
    .help("Set speed")
    .default_value("42");
# }
```
