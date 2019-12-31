## MagicalMethods

They are the reason why `structopt` is so easy to use and convenient in most cases.
Many of them have defaults, some of them get used even if not mentioned.

Methods may be used on "top level" (on top of a `struct`, `enum` or `enum` variant)
and/or on "field-level" (on top of a `struct` field or *inside* of an enum variant).
Top level (non-magical) methods correspond to `App::method` calls, field-level methods
are `Arg::method` calls.

```ignore
#[structopt(top_level)]
struct Foo {
    #[structopt(field_level)]
    field: u32
}

#[structopt(top_level)]
enum Bar {
    #[structopt(top_level)]
    Pineapple {
        #[structopt(field_level)]
        chocolate: String
    },

    #[structopt(top_level)]
    Orange,
}
```

- `name`: `[name = "name"]`
  - On top level: `App::new("name")`.

    The binary name displayed in help messages. Defaults to the crate name given by Cargo.

  - On field-level: `Arg::with_name("name")`.

    The name for the argument the field stands for, this name appears in help messages.
    Defaults to a name, deduced from a field, see also
    [`rename_all`](#specifying-argument-types).

- `version`: `[version = "version"]`

    Usable only on top level: `App::version("version" or env!(CARGO_PKG_VERSION))`.

    The version displayed in help messages.
    Defaults to the crate version given by Cargo. If `CARGO_PKG_VERSION` is not
    set no `.version()` calls will be generated unless requested.

- `no_version`: `no_version`

    Usable only on top level. Prevents default `App::version` call, i.e
    when no `version = "version"` mentioned.

- `author`: `author [= "author"]`

    Usable only on top level: `App::author("author" or env!(CARGO_PKG_AUTHOR))`.

    Author/maintainer of the binary, this name appears in help messages.
    Defaults to the crate author given by cargo, but only when `author` explicitly mentioned.

- `about`: `about [= "about"]`

    Usable only on top level: `App::about("about" or env!(CARGO_PKG_DESCRIPTION))`.

    Short description of the binary, appears in help messages.
    Defaults to the crate description given by cargo,
    but only when `about` explicitly mentioned.

- [`short`](#specifying-argument-types): `short [= "short-opt-name"]`

    Usable only on field-level.

- [`long`](#specifying-argument-types): `long [= "long-opt-name"]`

    Usable only on field-level.

- [`defautl_value`](#default-values): `default_value [= "default value"]`

    Usable only on field-level.

- [`rename_all`](#specifying-argument-types):
    [`rename_all = "kebab"/"snake"/"screaming-snake"/"camel"/"pascal"/"verbatim"]`

    Usable both on top level and field level.

- [`parse`](#custom-string-parsers): `parse(type [= path::to::parser::fn])`

    Usable only on field-level.

- [`skip`](#skipping-fields): `skip [= expr]`

    Usable only on field-level.

- [`flatten`](#flattening): `flatten`

    Usable only on field-level.

- [`subcommand`](#subcommands): `subcommand`

    Usable only on field-level.

- [`env`](#environment-variable-fallback): `env [= str_literal]`

    Usable only on field-level.

- [`rename_all_env`](##auto-deriving-environment-variables):
    [`rename_all_env = "kebab"/"snake"/"screaming-snake"/"camel"/"pascal"/"verbatim"]`

    Usable both on top level and field level.

- [`verbatim_doc_comment`](#doc-comment-preprocessing-and-structoptverbatim_doc_comment):
    `verbatim_doc_comment`

    Usable both on top level and field level.
