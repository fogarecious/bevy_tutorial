# Unofficial Tutorial Of Bevy

[Bevy](https://bevyengine.org/) is a game engine built in [Rust](https://www.rust-lang.org/).
This tutorial serves as a quick start for [Bevy](https://bevyengine.org/).
We try to keep each part of the tutorial as simple as possible.

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
  * Too Many Parameters For Systems
    <!-- system_param.rs -->
    <!-- query and resource -->
    <!-- name, hp, with mp -->
* Faster Compile Time
  <!-- (remove in release) -->

## 2D Rendering

* Windows
  * An App With A Window
  * The Default Scheduler For Windowed App
  * Initializing A Different Window
    <!-- window_settings.rs -->
  * Changing The Window After Initialization
    <!-- window_settings.rs -->
  * Low-Power Windows
    <!-- low_power.rs -->
  * Closing The Window On Esc Pressed
* Camera 2D
  <!-- (origin, positive x y) -->
* The Background Color
  <!-- clear_color.rs -->
* Images
  * Displaying Images
  * Obtaining Sizes Of Images
  * Resizing Images
* Transformation
  * Translation
  * Rotation
  * Scale
  * Combining Multiple Transformation
* Texts
  * Displaying Texts
  * Text Anchors
  * Font And Styles
  <!-- text2d.rs -->
* Shapes
  * Circles
  * Quads
  * RegularPolygons
  <!-- bevy::prelude::shape -->
* Materials
  * Colors
  * Textures
    <!-- diff shapes -->

## Input

* Keyboards
  * Keyboard Input
  * Keyboard Events
* Mouses
  * Mouse Input
  * Mouse Events
    <!-- cursor -->
* Timers
  <!-- time, w/ w/o repeat, fixed_timestep.rs -->
* Triggering An Event
  <!-- exit -->
* Custom Events
  <!-- event.rs -->

## States

* Turning On/Off A System
  <!-- run_if -->
* Using The State Machine
* Changing States
* Monitoring State Transition
<!-- generic_system.rs -->
<!-- derive States -->
<!-- enum AppState -->
<!-- add_state::<AppState>() -->
<!-- add_systems(OnExit(AppState::MainMenu), ...) -->
<!-- OnEnter, OnExit, OnTransition -->
<!-- run_if(in_state(AppState::MainMenu)) -->
<!-- ResMut<NextState<AppState>> -->
<!-- NextState, next_state.set -->

## 3D Rendering

* Camera 3D
  <!-- camera position, look at -->
* Shapes
  <!-- different shapes -->
* Transformation
  <!-- parent/children, despawn recursively -->
* Lighting
* Physically Based Rendering
* Fog
* Skipping The White Window
  <!-- window_settings.rs -->
<!-- ? multiple windows/cameras, camera background, 2d+3d -->

<!-- * User Interfaces -->

<!-- * Debug -->
  <!-- * Using Local Variables In Systems
  15, SystemParamFunction -->
  <!-- gizmos -->
  <!-- 2d_gizmos.rs -->

<!-- animation -->
  <!-- sprite_sheet.rs -->
<!-- audio -->
<!-- plugin -->
<!-- wasm, my practice, 26_wasm_test -->
<!-- beyond bevy -->

## See Also

* [Bevy](https://github.com/bevyengine/bevy) - the GitHub of the Bevy game engine.
* [Bevy Cheat Book](https://bevy-cheatbook.github.io/) - a reference-style book for the Bevy game engine.

## Contributions

Pull requests for typos, incorrect information, or other ideas are welcome!

## License

All code in the tutorial is provided under the MIT License.
