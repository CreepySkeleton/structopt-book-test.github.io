## Environment variable fallback

It is possible to specify an environment variable fallback option for an arguments
so that its value is taken from the specified environment variable if not
given through the command-line:

```
# use structopt::StructOpt;

#[derive(StructOpt)]
struct Foo {
  #[structopt(short, long, env = "PARAMETER_VALUE")]
  parameter_value: String
}
# fn main() {}
```

By default, values from the environment are shown in the help output (i.e. when invoking
`--help`):

```shell
$ cargo run -- --help
...
OPTIONS:
  -p, --parameter-value <parameter-value>     [env: PARAMETER_VALUE=env_value]
```

In some cases this may be undesirable, for example when being used for passing
credentials or secret tokens. In those cases you can use `hide_env_values` to avoid
having structopt emit the actual secret values:
```
# use structopt::StructOpt;

#[derive(StructOpt)]
struct Foo {
  #[structopt(long = "secret", env = "SECRET_VALUE", hide_env_values = true)]
  secret_value: String
}
```
