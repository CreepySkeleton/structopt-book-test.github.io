### Doc comment preprocessing and `#[structopt(verbatim_doc_comment)]`

`structopt` applies some preprocessing to doc comments to ease the most common uses:

* Strip leading and trailing whitespace from every line, if present.

* Strip leading and trailing blank lines, if present.

* Interpret each group of non-empty lines as a word-wrapped paragraph.

  We replace newlines within paragraphs with spaces to allow the output
  to be re-wrapped to the terminal width.

* Strip any excess blank lines so that there is exactly one per paragraph break.

* If the first paragraph ends in exactly one period,
  remove the trailing period (i.e. strip trailing periods but not trailing ellipses).

Sometimes you don't want this preprocessing to apply, for example the comment contains
some ASCII art or markdown tables, you would need to preserve LFs along with
blank lines and the leading/trailing whitespace. You can ask `structopt` to preserve them
via `#[structopt(verbatim_doc_comment)]` attribute.

**This attribute must be applied to each field separately**, there's no global switch.


<div class="important block">

**Important:** Keep in mind that `structopt` will *still* remove one leading space from each
line, even if this attribute is present, to allow for a space between
`///` and the content.

Also, `structopt` will *still* remove leading and trailing blank lines so
these formats are equivalent:

```ignore
/** This is a doc comment

Hello! */

/**
This is a doc comment

Hello!
*/

/// This is a doc comment
///
/// Hello!
```

</div>

[`App::about`]:      https://docs.rs/clap/2/clap/struct.App.html#method.about
[`App::long_about`]: https://docs.rs/clap/2/clap/struct.App.html#method.long_about
[`Arg::help`]:       https://docs.rs/clap/2/clap/struct.Arg.html#method.help
[`Arg::long_help`]:  https://docs.rs/clap/2/clap/struct.Arg.html#method.long_help
