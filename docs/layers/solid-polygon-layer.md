<p class="badges">
  <img src="https://img.shields.io/badge/64--bit-support-blue.svg?style=flat-square" alt="64-bit" />
  <img src="https://img.shields.io/badge/extruded-yes-blue.svg?style=flat-square" alt="extruded" />
</p>

# SolidPolygonLayer

The SolidPolygon Layer renders filled polygons.

```js
import DeckGL, {SolidPolygonLayer} from 'deck.gl';

new PolygonLayer({
  data: [
    [[0, 0], [0, 1], [1, 1], [1, 0], [0, 0]],   // Simple polygon (array of coords)
    [                                           // Complex polygon with one hole
      [[0, 0], [0, 2], [2, 2], [2, 0], [0, 0]], // (array of array of coords)
      [[0, 0], [0, 1], [1, 1], [1, 0], [0, 0]]
    ]
  ]
});
```

* The polypgons can be simple or complex (complex polygons are polygons with holes).
* A simple polygon specified as an array of vertices, each vertice being an array
  of two or three numbers
* A complex polygon is specified as an array of simple polygons, the
  first polygon representing the outer outline, and the remaining polygons
  representing holes. These polygons are expected to not intersect.

## Properties

Inherits from all [Base Layer](/docs/api-reference/layer.md) properties.

### Render Options

##### `filled` (Boolean, optional)

* Default: `true`

Whether to fill the polygons (based on the color provided by the
`getFillColor` accessor.

##### `extruded` (Boolean, optional)

* Default: `false`

Whether to extrude the polygons (based on the elevations provided by the
`getElevation` accessor. If set to false, all polygons will be flat, this
generates less geometry and is faster than simply returning `0` from `getElevation`.

##### `wireframe` (Boolean, optional)

* Default: `false`

Whether to generate a line wireframe of the hexagon. The outline will have
"horizontal" lines closing the top and bottom polygons and a vertical line
(a "strut") for each vertex on the polygon.

##### `elevationScale` (Number, optional)

* Default: `1`

Elevation multiplier. The final elevation is calculated by
  `elevationScale * getElevation(d)`. `elevationScale` is a handy property to scale
all elevation without updating the data.

##### `fp64` (Boolean, optional)

* Default: `false`

Whether the layer should be rendered in high-precision 64-bit mode. Note that since deck.gl v6.1, the default 32-bit projection uses a hybrid mode that matches 64-bit precision with significantly better performance.

**Remarks:**

* These lines are rendered with `GL.LINE` and will thus always be 1 pixel wide.
* Wireframe and solid extrusions are exclusive, you'll need to create two layers
  with the same data if you want a combined rendering effect.

##### `lightSettings` (Object, optional) **EXPERIMENTAL**

This is an object that contains light settings for extruded polygons.
Be aware that this prop will likely be changed in a future version of deck.gl.

### Data Accessors

##### `getPolygon` (Function, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `object => object.polygon`

Like any deck.gl layer, the polygon accepts a data prop which is expected to
be an iterable container of objects, and an accessor
that extracts a polygon (simple or complex) from each object.

This accessor returns the polygon corresponding to an object in the `data` stream.

##### `getFillColor` (Function|Array, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `[0, 0, 0, 255]`

The rgba fill color of each object's polygon, in `r, g, b, [a]`. Each component is in the 0-255 range.

* If an array is provided, it is used as the fill color for all polygons.
* If a function is provided, it is called on each polygon to retrieve its fill color.

##### `getLineColor` (Function|Array, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `[0, 0, 0, 255]`

The rgba stroke color of each object's polygon, in `r, g, b, [a]`. Each component is in the 0-255 range.

* If an array is provided, it is used as the stroke color for all polygons.
* If a function is provided, it is called on each object to retrieve its stroke color.

##### `getElevation` (Function|Number, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `1000`

The elevation of each object's polygon.

If a cartographic projection mode is used, height will be interpreted as meters,
otherwise will be in unit coordinates.

* If a number is provided, it is used as the elevation for all polygons.
* If a function is provided, it is called on each object to retrieve its elevation.

## Remarks

* This layer only renders filled polygons. If you need to render polygon
  outlines, use the [`PathLayer`](/docs/layers/path-layer.md)
* Polygons are always closed, i.e. there is an implicit line segment between
  the first and last vertices, when those vertices are not equal.
* The specification of complex polygons intentionally follows the GeoJson
  conventions for representing polugons with holes.

## Source

[modules/layers/src/solid-polygon-layer](https://github.com/uber/deck.gl/tree/6.2-release/modules/layers/src/solid-polygon-layer)

