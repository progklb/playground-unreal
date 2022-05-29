# Starter Course

All assets here are part of the **Unreal Engine 5 Beginner Tutorial - UE5 Starter Course!**

This is a crash-course for developers new to Unreal.

https://www.youtube.com/watch?v=gQmiqmxJMtA

## Sub-projects

- **MaterialTest**: Demonstrates creation of materials using the material node editor. A master material has been created, with a derived material instances. A static mesh (crate) as also been configured.

- **LightingTest**: Demonstrates lighting in a scene using Lumen. The coloured walls allow us to see the bounce lighting behaviour.

- **LightingExample**:
  - Maps/ArchVizRoom: Showcases a near-default setup of components for realistic lighting.

## Notes

### Typical Project Structure

Based on the UE5 starter content, and other examples.

```txt
+-- Content
|   +-- Project
|   |   +-- Audio
|   |   +-- Blueprints
|   |   +-- HDRI
|   |   +-- Maps
|   |   +-- Materials
|   |   +-- Meshes
|   |   +-- Particles
|   |   +-- Textures
```

### Useful Keyboard Shortcuts

- `End`: Snaps an object to a surface.

- `Ctrl+E`: Opens object for editing.
- `Ctrl+Space`: Opens content drawer.


### Unreal Concepts

- Actors: Items in the scene. Are made up of components. e.g. Physics cube.
- Components: Provide functionality to actors. e.g. Physics cube - mesh renderer, collision shape, audio, ...

- Blueprints: Not just code! These are like Unity prefabs where you can set up mini scenes housing logic etc. Blueprints are customizable, whereas built-in static meshes etc are not, so it is necessary to create a blueprint and then add you static mesh etc if you want to have custom functionality (e.g. moving the mesh around).

### Migrating Assets Between Projects

1. Open both projects in the Editor.
2. Right-click on assets to move > Migrate.
3. A popup will show all assets selected for export > Click okay, and choose the target folder.

### Materials

- Materials can be created using the materials graph.
- Create new basic material via right-click > Materials > Material.
- Build material behaviour using node graph.
  - *Note that this material will need to be recompiled for every change, which can be tedious. Use parameters on **material instances** to avoid this.*

- Create a **material instance** by selecting a material, right-click > Create material insance.
- Assign the material instance to your object.
- In the original material, parameterise values by right-clicking the nodes > Convert to parameter, and provide a name.
- Changes to parameters on the instance will now apply in real-time.

> **Master materials**
>
> A term given to a material that is used as the base for various instances. The master can expose a number of common properties, and each **instance** can then represent a variation (e.g. different textures / colours for grass, stone, etc. based on the same master material)

> **Reversed shadows***
>
> UE uses DX normal maps, but some modelling software use OpenGL normal maps.
> If an OpenGL map is used, shadow direction will be inverted.
> Fix by inspecting the normal map, and toggling "Flip Green Channel".

> **Imports**
>
> For **normal maps**, **metallic maps**, and **roughness maps** ensure the sRGB flag is disabled. This is because each channel is a separate mask, and these files are not supposed to be read as combined RGB.

> See project: MaterialTest

### Static Meshes

- May not have materials assigned by default. To assign, open in the mesh view and directly assign the materials to the mesh.

### Lighting

> Note that there are separate settings between edit mode and play mode. If the lighting doesn't look right in either mode, or each mode looks different, check your auto exposure settings for each mode.

Lumen - Allows global illumination (light bouncing) in real-time. This avoids having to do light-baking, and gives us far more dynamic scene lighting.

> Baked lighting is always better quality though and cheaper, so if lights/objects are static rather use baking.

**GI in UE4**: Generated a static light map.

- Set light source to Mobility:static.
- Buid > Build All Levels to build lighting.

**GI in UE5**: Lumen

- World Settings > Lightmass > Force No Precomputed Lighting: true
- Rebuild
- Enable Lumen within a PostProcessVolume:
  - Global Illumination > Method > Lumen
  - Reflections > Method > Lumen
- "Final Gather Quality" affects lighting quality. Lower values are noisier, but less expensive.

**Components**

- `Skylight`: captures an image of the scene and projects it as a lightsource. Acts like a dynamic HDRI image.
  - To realistically light an outdoor scene, combine this with a `SkyAtmoshere` component and `DirectionalLight`, with the following settings to achieve a natural ambient ligtning based on the scene atmosphere.
    - `SkyLight`:
      - Light > Real Time Capture
      - Mobility > Moveable
    - `Direcional Light`:
      - Atmosphere and Cloud > Atmosphere Sun Light: true

> Note that a `SkyAtmoshphere` will only render a sky from the horizon upward. To get rid of the black zone below the horizon, add an `ExponentialHeightFog`.

> Note that if a light does not point toward an indoor spac (e.g. a directional light does not point towards a window) that space will be pitch black.

**Making a convincing scene:**

- Add `DirectionalLight`.
  - Set Atmosphere Sun Light > true.
- Add `SkyAtmosphere`. 
- Add `ExponentialHeightFog`.
  - Consider adjusting Start Distance so that the playable area doesn't have fog in it.
- Add `SkyLight`
  - Set Mobility > Moveable
  - Set Light > Real Time Capture
  - Adjust Intensity Scale to change brightness
- Add `PostProcessingVolume`
  - Set Infinite Extent (Unbound) > true
  - Set Lumen Global Illumination > Final Gather Quality > Max value

> See project: LightingExample/ArchVizRoom