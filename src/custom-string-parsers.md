## Custom string parsers

If the field type does not have a `FromStr` implementation, or you would
like to provide a custom parsing scheme other than `FromStr`, you may
provide a custom string parser using `parse(...)` like this:

```
# use structopt::StructOpt;
# fn main() {}
use std::num::ParseIntError;
use std::path::PathBuf;

fn parse_hex(src: &str) -> Result<u32, ParseIntError> {
    u32::from_str_radix(src, 16)
}

#[derive(StructOpt)]
struct HexReader {
    #[structopt(short, parse(try_from_str = parse_hex))]
    number: u32,
    #[structopt(short, parse(from_os_str))]
    output: PathBuf,
}
```

There are five kinds of custom parsers:

| Kind              | Signature                             | Default                         |
|-------------------|---------------------------------------|---------------------------------|
| `from_str`        | `fn(&str) -> T`                       | `::std::convert::From::from`    |
| `try_from_str`    | `fn(&str) -> Result<T, E>`            | `::std::str::FromStr::from_str` |
| `from_os_str`     | `fn(&OsStr) -> T`                     | `::std::convert::From::from`    |
| `try_from_os_str` | `fn(&OsStr) -> Result<T, OsString>`   | (no default function)           |
| `from_occurrences`| `fn(u64) -> T`                        | `value as T`                    |
| `from_flag`       | `fn(bool) -> T`                       | `::std::convert::From::from`    |

The `from_occurrences` parser is special. Using `parse(from_occurrences)`
results in the _number of flags occurrences_ being stored in the relevant
field or being passed to the supplied function. In other words, it converts
something like `-vvv` to `3`. This is equivalent to
`.takes_value(false).multiple(true)`. Note that the default parser can only
be used with fields of integer types (`u8`, `usize`, `i64`, etc.).

The `from_flag` parser is also special. Using `parse(from_flag)` or
`parse(from_flag = some_func)` will result in the field being treated as a
flag even if it does not have type `bool`.

When supplying a custom string parser, `bool` will not be treated specially:

Type        | Effect            | Added method call to `clap::Arg`
------------|-------------------|--------------------------------------
`Option<T>` | optional argument | `.takes_value(true).multiple(false)`
`Vec<T>`    | list of arguments | `.takes_value(true).multiple(true)`
`T`         | required argument | `.takes_value(true).multiple(false).required(!has_default)`

In the `try_from_*` variants, the function will run twice on valid input:
once to validate, and once to parse. Hence, make sure the function is
side-effect-free.
