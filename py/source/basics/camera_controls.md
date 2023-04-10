These settings affect the 3D camera view in polyscope. It is often convenient to set them just before calling `polyscope.init()`, but they may generally be set anywhere.

```python
import polyscope as ps
import numpy as np

ps.set_up_dir("z_up")
ps.set_front_dir("neg_y_front")
ps.set_navigation_style("free")

# initialize
ps.init()
   
# set the camera pose explicitly
ps.look_at((0., 0., 5.), (1., 1., 1.))

# (alternately, use numpy vectors)
ps.look_at(np.array((0., 0., 5.)), np.array((1., 1., 1.)))

# show the GUI
ps.show()
```

### Options

??? func "`#!python set_navigation_style(s)`"

    ##### navigation style
   
    The style of the camera navigation. Affects what happens when you drag to rotate around the 3D view with your mouse.

    This value can be manually set under the `view` menu of the ui. Programmatically, pass a string for the following settings:

    - `'turntable'` The up direction (see below) is always fixed vertically, with rotation along the azumith and altitude directions.
    - `'free'` The camera is free to take any orientation, rotation is always about relative to the current camera.
    - `'planar'` The camera is locked in to a 2D view of the XY plane, with no rotation (see [2D data]([[url.prefix]]/features/2D_data)).

    Default: `'turntable'`.

    Example:
    ```python
    import polyscope as ps
    ps.set_navigation_style("free")

??? func "`#!python get_navigation_style()`"

    Get the current navigation style (see explanation in the setter above). Returns a string.


??? func "`#!python set_up_dir(s)`"

    ##### up direction
    
    Set the default "up" direction for the scene. This setting affects default orientation of the view, the behavior of some navigation styles (esp. `Turntable`), and the orientation of the ground plane.

    This value can be manually set under the `view` menu of the ui. Programmatically, the setting strings are:

    - `'x_up'` The positive X-axis is up.
    - `'neg_x_up'` The negative X-axis is up.
    - `'y_up'` The positive Y-axis is up.
    - `'neg_y_up'` The negative Y-axis is up.
    - `'z_up'` The positive Z-axis is up.
    - `'neg_z_up'` The negative Z-axis is up.
   
    Default: `'y_up'`.

    Example:
    ```python
    import polyscope as ps
    ps.set_up_dir("z_up")
    ```

??? func "`#!python get_up_dir()`"

    Get the current up direction (see explanation in the setter above). Returns a string.

??? func "`#!python set_front_dir(s)`"

    ##### front direction

    Set the default "front" direction for the scene. This setting affects default orientation of the view; the starting camera looks at the front of the data.

    This value is interpeted as if the scene were a cube, and you are specifying which face of the cube is the 'front'. So `z_front` means the +Z face of the cube is the front face, and thus our camera initially points down the -Z axis to look at it.

    This value can be manually set under the `view` menu of the ui. Programmatically, the setting strings are:

    - `'x_front'` The positive X-axis is the front.
    - `'neg_x_front'` The negative X-axis is the front.
    - `'y_front'` The positive Y-axis is the front.
    - `'neg_y_front'` The negative Y-axis is the front.
    - `'z_front'` The positive Z-axis is the front.
    - `'neg_z_front'` The negative Z-axis is the front.

   
    Default: `z_front`.

    Example:
    ```cpp
    import polyscope as ps
    ps.set_front_dir("z_front")
    ```

??? func "`#!python get_front_dir()`"

    Get the current front direction (see explanation in the setter above). Returns a string.


??? func "`#!python look_at(camera_location, target, fly_to=False)`"

    ##### look at

    Set the camera to be located at the 3D position `camera_location` and looking at the 3D position `target`, both in world coordinates. The up direction for the camera is set to be the scene's up direction. If `fly_to=True`, the camera will smoothly animate to the new configuration.

    The input 3D vectors can be tuples, length-3 numpy arrays, or really anything that can be indexed for three components.

    Example:
    ```python
    polyscope.look_at((0., 0., 5.), (1., 1., 1.))
    ```

??? func "`#!python look_at_dir(camera_location, target, up_dir, fly_to=False)`"
    
    Set the camera to be located at the 3D position `camera_location` and looking at the 3D position `target`, oriented with the up direction `up_dir`, all in world coordinates. If `fly_to=True`, the camera will smoothly animate to the new configuration.

    Note that setting the up direction for the camera view with this function is separate from the scene's up direction with `set_up_dir()`, which affects things like ground plane placement, and manual view manipulation.

    The input 3D vectors can be tuples, length-3 numpy arrays, or really anything that can be indexed for three components.

    Example:
    ```python
    polyscope.look_at_dir((0., 0., 5.), (1., 1., 1.), (-1., -1., 0.))
    ```

??? func "`#!python reset_camera_to_home_view()`"

    ##### reset camera to home view

    Reset the camera view to the home view (a reasonable default view scaled to the scene).

    **Note:** The "home" view is dependent on the data in the scene; it is computed from the bounding boxes of all registered structures to ensure that everything is nicely scaled and in view. As such, one should generally call this function _after_ registering data.

    Example:
    ```python
    ps.reset_camera_to_home_view()
    ```

#### Orthographic view

By default, Polyscope's view uses [perspective projection](https://en.wikipedia.org/wiki/3D_projection#Perspective_projection). Perspective projections roughly correspond to how images are usually perceived by our eyes and cameras.

Alternately [orthographic projection](https://en.wikipedia.org/wiki/3D_projection#Orthographic_projection) is also supported. Orthographic projections are common in engineering and architecture, because they have the property that distances are preserved in the projected image, regardless of whether the object is near or far from the camera.

![image of perspective and orthographic projection]([[url.prefix]]/media/projection_diagram.jpg)

In perspective mode, zooming (for instance, by manually scrolling the mouse) translates the camera forward in space. In orthographic mode, it instead adjusts the field of view without moving the camera. This is because counter-intuitively, translating the camera forward does not actually change the view in an orthographic projection!


??? func "`#!python set_view_projection_mode(mode_name)`"

    ##### set projection mode

    Set the camera view projection to be either orthographic or perspective (default).

    Example:
    ```python
    # Enable the orthographic view
    ps.set_view_projection_mode("orthographic")

    # Go back to default perspective projection
    ps.set_view_projection_mode("perspective")
    ```
