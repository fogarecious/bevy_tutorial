# Setting Up

Initialize a [Cargo](https://doc.rust-lang.org/cargo/guide/) project.

```sh
cargo new my_project
```

where `my_project` is the name of our project.

Add [Bevy](https://bevyengine.org/) to the project dependencies.

```sh
cd my_project
cargo add bevy
```

You should see the dependency in the end of `Cargo.toml` file.

```toml
[dependencies]
bevy = "0.12.1"
```

Now, you are ready to exploit the power of [Bevy](https://bevyengine.org/).
