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
  * Quads
  * RegularPolygons
  * Shapes With Transformation
  <!-- bevy::prelude::shape -->
* Materials
  * Colors
  * Textures
    <!-- diff shapes, text? -->

## Input

* Keyboards
  * Keyboard Input
    <!-- Keyboard Input -->
    <!-- Keyboard Modifiers	 -->
    <!-- pressed, move object -->
    <!-- just_pressed, switch color  -->
    <!-- name modifier, released -->
  * Keyboard Events
    <!-- Keyboard Input Events -->
    <!-- series of events in a batch -->
* Mouses
  * Mouse Input
    <!-- Mouse Input -->
    <!-- pressed, change color -->
    <!-- just_pressed, switch color  -->
    <!-- name released -->
    * Mouse Events
      <!-- Mouse Input Events -->
      <!-- demo some, name all -->
  * Mouse Cursor
    <!-- Mouse Grab	 -->
    <!-- bevy::window::Cursor -->
* Timers
  <!-- time, w/ w/o repeat, fixed_timestep.rs -->
  <!-- simple image animation -->
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

<!-- animation -->
  <!-- sprite_sheet.rs -->

<!-- * User Interfaces -->

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
