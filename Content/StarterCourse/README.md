# Starter Course

All assets here are part of the **Unreal Engine 5 Beginner Tutorial - UE5 Starter Course!**

This is a crash-course for developers new to Unreal.

https://www.youtube.com/watch?v=gQmiqmxJMtA

## Sub-projects

- MaterialTest: Demonstrating creation of materials using the material node editor.

## Notes

### Typical Project Structure

Based on the UE5 starter content.

```txt
+-- Content
|   +-- Project
|   |   +-- Audio
|   |   +-- Blueprints
|   |   +-- HDRI
|   |   +-- Maps
|   |   +-- Materials
|   |   +-- Particles
|   |   +-- Props
|   |   +-- Shapes
|   |   +-- Textures
```

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

See sub-project: MaterialTest.