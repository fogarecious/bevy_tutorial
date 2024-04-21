# Easing

Previously, we use [Timer](https://docs.rs/bevy/latest/bevy/time/struct.Timer.html)s to move a shape smoothly (or periodically).
Sometimes, we need the movement to speed up or slow down.
This is often called *easing*.
A famous technique to perform easing is [Bezier curve](https://en.wikipedia.org/wiki/B%C3%A9zier_curve).

In the following example, we make a circle going up quickly and then going down slowly.
This is done by [CubicCurve](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicCurve.html).

We create a resource `MyCurve` that contains [CubicCurve](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicCurve.html)<[Vec3](https://docs.rs/bevy/latest/bevy/math/struct.Vec3.html)>.

```rust
#[derive(Resource)]
struct MyCurve(CubicCurve<Vec3>);

impl Default for MyCurve {
    fn default() -> Self {
        let points = [
            [
                vec3(0., -100., 0.),
                vec3(0., 200., 0.),
                vec3(0., 50., 0.),
                vec3(0., -100., 0.),
            ]
        ];

        let curve = CubicBezier::new(points).to_curve();

        Self(curve)
    }
}
```

To construct a [CubicCurve](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicCurve.html), we first initialize a [CubicBezier](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicBezier.html) and then turn the [CubicBezier](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicBezier.html) (by its method [to_curve()](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicBezier.html#method.to_curve)) to [CubicCurve](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicCurve.html).
The [CubicBezier](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicBezier.html) takes an array of sets of points in its initialization (`CubicBezier::new(points)` here).
Note that the variable `points` is an array of arrays, which means an array of point sets.
Each set of points should contain four points.
For simplicity, we use only one set of points here.

Next, we use the method [position](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicCurve.html#method.position)\(`t`) of [CubicCurve](https://docs.rs/bevy/latest/bevy/math/cubic_splines/struct.CubicCurve.html) to help us computing the actual position of the shape at time `t`.
Since we only have one set of points, the range of `t` is between 0 and 1.

```rust
fn circle_moves(
    time: Res<Time>,
    mut circles: Query<&mut Transform, With<Handle<ColorMaterial>>>,
    my_curve: Res<MyCurve>,
) {
    let mut transform = circles.single_mut();

    let t = time.elapsed_seconds().rem_euclid(2.) / 2.;
    
    transform.translation = my_curve.0.position(t);
}
```

The method [rem_euclid](https://doc.rust-lang.org/std/primitive.f32.html#method.rem_euclid)\(`2.`) of [f32](https://doc.rust-lang.org/std/primitive.f32.html) returns a number between 0 and 2.
`x.rem_euclid(2.)` means the position after walking on a circle with circumference `2` for distance `x`.
For example, if we walk 1, it returns 1.
If we walk 2, it returns 0.
If we walk 2.5, it returns 0.5.
And if we walk 3.5, it returns 1.5.
Finally, since `rem_euclid(2.)` returns a number between 0 and 2, we further divide it by 2 so that the new number is between 0 and 1, which is the acceptable range.

The full code is as follows:

```rust
use bevy::{
    app::{App, Startup, Update},
    asset::{Assets, Handle},
    core_pipeline::core_2d::Camera2dBundle,
    ecs::{
        query::With,
        system::{Commands, Query, Res, ResMut, Resource},
    },
    math::{
        cubic_splines::{CubicBezier, CubicCurve, CubicGenerator},
        vec3, Vec3,
    },
    render::mesh::{shape::Circle, Mesh},
    sprite::{ColorMaterial, ColorMesh2dBundle},
    time::Time,
    transform::components::Transform,
    utils::default,
    DefaultPlugins,
};

#[derive(Resource)]
struct MyCurve(CubicCurve<Vec3>);

impl Default for MyCurve {
    fn default() -> Self {
        let points = [
            [
                vec3(0., -100., 0.),
                vec3(0., 200., 0.),
                vec3(0., 50., 0.),
                vec3(0., -100., 0.),
            ]
        ];

        let curve = CubicBezier::new(points).to_curve();

        Self(curve)
    }
}

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .init_resource::<MyCurve>()
        .add_systems(Startup, setup)
        .add_systems(Update, circle_moves)
        .run();
}

fn setup(mut commands: Commands, mut meshes: ResMut<Assets<Mesh>>) {
    commands.spawn(Camera2dBundle::default());

    commands.spawn(ColorMesh2dBundle {
        mesh: meshes.add(Circle::new(50.).into()).into(),
        ..default()
    });
}

fn circle_moves(
    time: Res<Time>,
    mut circles: Query<&mut Transform, With<Handle<ColorMaterial>>>,
    my_curve: Res<MyCurve>,
) {
    let mut transform = circles.single_mut();

    let t = time.elapsed_seconds().rem_euclid(2.) / 2.;
    
    transform.translation = my_curve.0.position(t);
}
```

By running the app, we can see that a circle first goes up quickly and then goes down slowly.
The movement repeats.

If we have two sets of points, the range of `t` will be between 0 and 2, and so on.

```rust
// ...

let points = [
    [
        vec3(0., -100., 0.),
        vec3(0., 200., 0.),
        vec3(0., 50., 0.),
        vec3(0., -100., 0.),
    ],
    [
        vec3(0., -100., 0.),
        vec3(200., -100., 0.),
        vec3(50., -100., 0.),
        vec3(0., -100., 0.),
    ],
];

// ...

let t = time.elapsed_seconds().rem_euclid(2.);
transform.translation = my_curve.0.position(t);

// ...
```

:arrow_right:  Next: [Triggering An Event](./triggering_an_event.md)

:blue_book: Back: [Table of contents](./../README.md)
