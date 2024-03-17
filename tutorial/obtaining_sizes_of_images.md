# Obtaining Sizes Of Images

In the previous tutorial, our app [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn)s a [SpriteBundle](https://docs.rs/bevy/latest/bevy/sprite/struct.SpriteBundle.html) and provides a [Handle](https://docs.rs/bevy/latest/bevy/asset/enum.Handle.html) for the [SpriteBundle](https://docs.rs/bevy/latest/bevy/sprite/struct.SpriteBundle.html)'s [texture](https://docs.rs/bevy/latest/bevy/sprite/struct.SpriteBundle.html#structfield.texture).
We can access this [Handle](https://docs.rs/bevy/latest/bevy/asset/enum.Handle.html) after it is spawned.

Since our [Handle](https://docs.rs/bevy/latest/bevy/asset/enum.Handle.html) points to an [Image](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html), we will query `&Handle<Image>` in our system.
The image data is stored in the resource [Assets](https://docs.rs/bevy/latest/bevy/asset/struct.Assets.html) and can be accessed by `Res<Assets<Image>>`.
We will use the `&Handle<Image>` to find the [Image](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html) stored in `Res<Assets<Image>>`.

```rust
fn print_image_size(image_handles: Query<&Handle<Image>>, images: Res<Assets<Image>>) {
    let image_handle = image_handles.single();

    let Some(image) = images.get(image_handle) else {
        return;
    };

    println!("{}", image.size());
}
```

In this example, the app [spawn](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Commands.html#method.spawn)s exactly one `Handle<Image>`, so we can use [single()](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html#method.single) of [Query](https://docs.rs/bevy/latest/bevy/ecs/system/struct.Query.html) to access the [Handle](https://docs.rs/bevy/latest/bevy/asset/enum.Handle.html).

If the [Image](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html) is ready to be drawn, `images.get(image_handle)` returns [Some](https://doc.rust-lang.org/std/option/enum.Option.html#variant.Some), otherwise it returns [None](https://doc.rust-lang.org/std/option/enum.Option.html#variant.None).

After we obtained the [Image](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html) as a variable, we can access its attributes, such as [size()](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html#method.size), [width()](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html#method.width), [height()](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html#method.height), [aspect_ratio()](https://docs.rs/bevy/latest/bevy/render/texture/struct.Image.html#method.aspect_ratio), etc.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup, Update},
    asset::{AssetServer, Assets, Handle},
    core_pipeline::core_2d::Camera2dBundle,
    ecs::system::{Commands, Query, Res},
    render::texture::Image,
    sprite::SpriteBundle,
    utils::default,
    DefaultPlugins,
};

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Startup, setup)
        .add_systems(Update, print_image_size)
        .run();
}

fn setup(mut commands: Commands, asset_server: Res<AssetServer>) {
    commands.spawn(Camera2dBundle::default());

    commands.spawn(SpriteBundle {
        texture: asset_server.load("bevy_bird_dark.png"),
        ..default()
    });
}

fn print_image_size(image_handles: Query<&Handle<Image>>, images: Res<Assets<Image>>) {
    let image_handle = image_handles.single();

    let Some(image) = images.get(image_handle) else {
        return;
    };

    println!("{}", image.size());
}
```

Output:

```text
[256, 256]
[256, 256]
[256, 256]
[256, 256]
[256, 256]
... (repeat infinitely)
```

<!-- :arrow_right:  Next:  -->

:blue_book: Back: [Table of contents](./../README.md)
