### Auto-deriving environment variables

Environment variables tend to be called after the corresponding `struct`'s field,
as in example above. The field is `secret_value` and the env var is "SECRET_VALUE";
the name is the same, except casing is different.

It's pretty tedious and error-prone to type the same name twice,
so you can ask `structopt` to do that for you.

```
# use structopt::StructOpt;

#[derive(StructOpt)]
struct Foo {
  #[structopt(long = "secret", env)]
  secret_value: String
}
```

It works just like `#[structopt(short/long)]`: if `env` is not set to some concrete
value the value will be derived from the field's name. This is controlled by
`#[structopt(rename_all_env)]`.

`rename_all_env` works exactly as `rename_all` (including overriding)
except default casing is `SCREAMING_SNAKE_CASE` instead of `kebab-case`.
