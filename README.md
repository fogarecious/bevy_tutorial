# Unofficial Tutorial Of Bevy

[Bevy](https://bevyengine.org/) is a game engine built in [Rust](https://www.rust-lang.org/).
This tutorial serves as a quick start for [Bevy](https://bevyengine.org/).
We try to keep each part of the tutorial as simple as possible.

(Currently, this tutorial is for version 0.12.1.)

## Basic

* [Setting Up](./tutorial/setting_up.md)
* [A Bevy Project That Does Nothing](./tutorial/a_bevy_project_that_does_nothing.md)
* Systems
  * [They Are Functions In Bevy](./tutorial/they_are_functions_in_bevy.md)
  * [The Default Scheduler For Systems](./tutorial/the_default_scheduler_for_systems.md)
  * [Executing Multiple Systems Simultaneously](./tutorial/executing_multiple_systems_simultaneously.md)
  * [Executing Multiple Systems In Order](./tutorial/executing_multiple_systems_in_order.md)
  * [Executing Multiple Systems Set By Set](./tutorial/executing_multiple_systems_set_by_set.md)
* Resources
  * [They Are Singleton Structs](./tutorial/they_are_singleton_structs.md)
  * [Updating Resources](./tutorial/updating_resources.md)
  * [Removing Resources](./tutorial/removing_resources.md)
* Entities And Components
  * [They Are Like Tables](./tutorial/they_are_like_tables.md)
  * [Searching For Entities By Components](./tutorial/searching_for_entities_by_components.md)
  * [Entities Can Have Different Components](./tutorial/entities_can_have_different_components.md)
  * [Bundles Help Us Grouping Components Together](./tutorial/bundles_help_us_grouping_components_together.md)
  * [Searching For Entities By Optional Components](./tutorial/searching_for_entities_by_optional_components.md)
  * [Searching For Entities With Filters](./tutorial/searching_for_entities_with_filters.md)
  * [Searching For The Only Entity](./tutorial/searching_for_the_only_entity.md)
  * [Searching And Updating Entities](./tutorial/searching_and_updating_entities.md)
  * [Searching And Removing Entities](./tutorial/searching_and_removing_entities.md)
  * [Removing Entities Directly](./tutorial/removing_entities_directly.md)
* Too Many Parameters
  * [Too Many Type Parameters For Queries](./tutorial/too_many_type_parameters_for_queries.md)
  * [Too Many Parameters For Systems](./tutorial/too_many_parameters_for_systems.md)
* [Too Many Systems](./tutorial/too_many_systems.md)
* [Faster Compile Time](./tutorial/faster_compile_time.md)

## 2D Rendering

* Windows
  * [An App With A Window](./tutorial/an_app_with_a_window.md)
  * [The Default Scheduler For Windowed App](./tutorial/the_default_scheduler_for_windowed_app.md)
  * [Initializing A Different Window](./tutorial/initializing_a_different_window.md)
  * [Changing The Window After Initialization](./tutorial/changing_the_window_after_initialization.md)
  * [Low-Power Windows](./tutorial/low_power_windows.md)
  * [Closing The Window On Esc Pressed](./tutorial/closing_the_window_on_esc_pressed.md)
* [Camera 2D](./tutorial/camera_2d.md)
* [The Background Color](./tutorial/the_background_color.md)
* Images
  * [Displaying Images](./tutorial/displaying_images.md)
  * [Obtaining Sizes Of Images](./tutorial/obtaining_sizes_of_images.md)
  * [Resizing Images](./tutorial/resizing_images.md)
* Transformation
  * [Translation](./tutorial/translation.md)
  * [Rotation](./tutorial/rotation.md)
  * [Scale](./tutorial/scale.md)
  * [Combining Multiple Transformation](./tutorial/combining_multiple_transformation.md)
* Texts
  * [Displaying Texts](./tutorial/displaying_texts.md)
  * [Font Styles](./tutorial/font_styles.md)
  * [Text Positions](./tutorial/text_positions.md)
* Shapes
  * [Circles](./tutorial/circles.md)
  * [Quads](./tutorial/quads.md)
  * [Regular Polygons](./tutorial/regular_polygons.md)
  * [Shapes With Transformation](./tutorial/shapes_with_transformation.md)
* Materials
  * [Colors](./tutorial/colors.md)
  * [Textures](./tutorial/textures.md)

## Input

* Keyboards
  * [Keyboard Input](./tutorial/keyboard_input.md)
  * [Keyboard Just Input](./tutorial/keyboard_just_input.md)
  * [Keyboard Events](./tutorial/keyboard_events.md)
* Mouses
  * [Mouse Input](./tutorial/mouse_input.md)
  * [Mouse Events](./tutorial/mouse_events.md)
  * [Mouse Icon](./tutorial/mouse_icon.md)
* Timers
  * [Engine Time](./tutorial/engine_time.md)
  * A Timer Running Once
    <!-- Timer, Timer::from_seconds(20.0, TimerMode::Once) -->
    <!-- timer.tick(time.delta()).just_finished() -->
    <!-- time up and scale -->
  * A Timer Running Repeatedly
    <!-- Timer, Timer::from_seconds(4.0, TimerMode::Repeating) -->
    <!-- timer.tick(time.delta()).just_finished() -->
    <!-- discrete rotation -->
  <!-- Timers -->
* Triggering An Event
  <!-- bevy::app::AppExit -->
  <!-- system, mut my_events: EventWriter<AppExit> -->
  <!-- my_events.send(AppExit); -->
  <!-- Res<Time> for 3 seconds, exit -->
* Custom Events
  <!-- event.rs (Event) -->
  <!-- EventWriter -->
  <!-- #[derive(Event)] -->
  <!-- App, add_event::<MyEvent>() -->
  <!-- system, mut my_events: EventWriter<MyEvent> -->
  <!-- my_events.send(MyEvent { ... }); -->
  <!-- mut events: EventReader<MyEvent> -->
  <!-- for my_event in events.read() -->
  <!-- in 2~3 seconds, rotate polygon -->

## States

* Turning On/Off A System
  <!-- Run Conditions	 -->
  <!-- run_if -->
* Using The State Machine
* Changing States
* Monitoring State Transition
<!-- generic_system.rs (Generic System) -->
<!-- or State -->
<!-- derive States -->
<!-- #[derive(Debug, Default, Clone, Copy, Eq, PartialEq, Hash, States)] -->
<!-- enum AppState, derive -->
<!-- #[default] in enum AppState -->
<!-- App, add_state::<AppState>() -->
<!-- add_systems(OnExit(AppState::MainMenu), ...) -->
<!-- OnEnter, OnExit, OnTransition -->
<!-- run_if(in_state(AppState::MainMenu)) -->
<!-- ResMut<NextState<AppState>> -->
<!-- NextState, next_state.set -->
<!-- or see Virtual time for key controlling systems -->

<!-- animation -->
  <!-- use Time to move shapes, see Fixed Timestep -->
  <!-- sprite_sheet.rs -->

<!-- * User Interfaces -->

## 3D Rendering

* Camera 3D
  <!-- Camera3dBundle -->
  * Camera Positions And Directions
    <!-- camera position, look at, default position -->
* Shapes
  * Cube
  * Box
  * UVSphere
  * Icosphere
  * Cylinder
  * Capsule
  * Torus
  * Plane
  <!-- bevy::prelude::shape -->
* Transformation
  * 3D Transformation
  * Hierarchical Transformation
    <!-- parent/children, despawn recursively -->
* Lighting
* Physically Based Rendering
* Fog
* Skipping The White Window
  <!-- window_settings.rs -->
<!-- ? multiple windows/cameras, camera background, 2d+3d -->
<!-- draw on images -->

<!-- * Debug -->
  <!-- * Using Local Variables In Systems
  15, SystemParamFunction -->
  <!-- gizmos -->
  <!-- 2d_gizmos.rs -->

<!-- audio -->
<!-- wasm, my practice, 26_wasm_test -->
<!-- beyond bevy -->

## See Also

* [Bevy](https://github.com/bevyengine/bevy) - the GitHub of the Bevy game engine.
* [Bevy Cheat Book](https://bevy-cheatbook.github.io/) - a reference-style book for the Bevy game engine.

## Contributions

Pull requests for typos, incorrect information, or other ideas are welcome!

## License

All code in the tutorial is provided under the MIT License.
