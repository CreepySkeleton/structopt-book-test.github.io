# Raw methods

They are the reason why `structopt` is so flexible.

Each and every method from `clap::App` and `clap::Arg` can be used directly -
just `#[structopt(method_name = single_arg)]` or `#[structopt(method_name(arg1, arg2))]`
and it just works. As long as `method_name` is not one of the magical methods -
it's just a method call.

<div class="note block">

**Note:** Raw methods are direct replacement for pre-0.3 structopt's
`#[structopt(raw(...))]` attributes, any time you would have used a `raw()` attribute
in 0.2 you should use raw method in 0.3.

Unfortunately, old raw attributes collide with `clap::Arg::raw` method. To explicitly
warn users of this change we allow `#[structopt(raw())]` only with `true` or `false`
literals (this method is supposed to be called only with `true` anyway).

</div>
