# Tiled support
> Tested in Tiled version `1.9.0`

[Video example](https://www.youtube.com/watch?v=hVCmLqZ0JVw)

Support for maps built with Tiled using the extension .json.

- [x] Multi TileLayer
- [x] Multi ObjectLayer
- [x] TileSet
- [x] Tile Animated
- [x] Load Map from URL
- [x] Text
- [x] Image Layer

Collision
   - [x] MultiCollision
   - [x] Retangle Collision
   - [x] Circle Collision
   - [x] Polygon Collision
   - [ ] Point Collision


## Using

Add the files generated by Tiled to the project by following the base: `assets/images/`

```yaml
flutter:
  assets:
    - assets/images/tiled/map.json
    - assets/images/tiled/tile_set.json
    - assets/images/tiled/img_tile_set.png
```

For maps built with Tiled we must use the Widget `BonfireWidget` (example [here](doc/getting-started?id=creating-your-map)):

```dart
  return BonfireWidget(
    joystick: Joystick(
      directional: JoystickDirectional()
    ),
    map: WorldMapByTiled(
      WorldMapReader.fromAsset('tiled/map.json'),
      forceTileSize: DungeonMap.tileSize, // if you want to force the size of the Tile to be larger or smaller than the original
      objectsBuilder: {
          'goblin': (TiledObjectProperties properties) => Goblin(properties.position),
          'torch': (TiledObjectProperties properties) => Torch(properties.position),
          'barrel': (TiledObjectProperties properties) => BarrelDraggable(properties.position,),
      },
    ),
  );
```

## Tiled map example

If you want the Tile to be drawn above the player add type: `above` in your tileSet.

![](../../_media/print_exemplo_tiled.png)

Result:

![](../../_media/print_result_tiled.png)


## Load map from network

You can store your map files in a server and load them. Just load using `TiledReader.network`:

```dart
  return BonfireWidget(
    joystick: Joystick(
      directional: JoystickDirectional()
    ),
    map: WorldMapByTiled(
      WorldMapReader.fromNetwork(
         Uri.parse('http://rafaelbarbosatec.github.io/tiled/my_map.json'),
       // cacheProvider: TiledMemoryCacheProvider()
      ),
    ),
  );
```


You can manage the cache of this too. By default, it uses `TiledMemoryCacheProvider`. You can create your own cache system only by creating a class and extending it by `TiledCacheProvider`.

## Useful

- You can set `class` in your tile to `above` to render this tile above all components in your game. 
- If you need all tiles of a layer rendered above all components you can create a `Custom Property` in your layer and create one named `class` with the value `above`.
- You can set `class` in your tile to `dynamicAbove` to render this tile dynamic by Y axis.
- You can set `class` in your Object to `collision`. This object will be added in the game with transparency and collision.
- You can set `class` in your ObjectLayer to `collision`. All objects of this layer will be added in the game with transparency and collision.
