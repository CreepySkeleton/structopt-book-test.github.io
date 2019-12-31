### `-h` vs `--help` (A.K.A `help()` vs `long_help()`)

The `-h` flag is not the same as `--help`.

-h corresponds to Arg::help/App::about and requests short "summary" messages
while --help corresponds to Arg::long_help/App::long_about and requests more
detailed, descriptive messages.

It is entirely up to `clap` what happens if you used only one of
[`Arg::help`]/[`Arg::long_help`], see `clap`'s documentation for these methods.

As of clap v2.33, if only a short message ([`Arg::help`]) or only
a long ([`Arg::long_help`]) message is provided, clap will use it
for both -h and --help. The same logic applies to `about/long_about`.

[`App::about`]:      https://docs.rs/clap/2/clap/struct.App.html#method.about
[`App::long_about`]: https://docs.rs/clap/2/clap/struct.App.html#method.long_about
[`Arg::help`]:       https://docs.rs/clap/2/clap/struct.Arg.html#method.help
[`Arg::long_help`]:  https://docs.rs/clap/2/clap/struct.Arg.html#method.long_help