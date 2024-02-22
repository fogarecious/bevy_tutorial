# Faster Compile Time

[Bevy](https://bevyengine.org/) is a considerable library that might require long compile time.
There is a way to speed the compiler up by adding the `dynamic_linking` feature.

The `Cargo.toml` file may look like this:

```toml
[dependencies]
bevy = { version = "0.12.1", features = ["dynamic_linking"] }
```

You might need to compile a [Bevy](https://bevyengine.org/) project without the `dynamic_linking` feature once, and then compile with the feature enabled for the following compilations to avoid compilation error.

However, `dynamic_linking` is not suitable in release mode.
So, please be sure to remove this feature when your project is ready to be released.

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)
