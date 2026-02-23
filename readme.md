# Plotly Axis Bounds Capability

Axis bounds define the visible minimum and maximum values of an axis (x or y). They control what portion of the data space is shown in the chart viewport.

In Plotly, axis bounds are typically set using:

- `range` → Explicit min/max values
- `autorange` → Automatic calculation based on data
- `fixedrange` → Locks user interaction (prevents zoom/pan)

## Setting Explicit Bounds

You can manually define the visible window:

```js
layout = {
  xaxis: {
    range: [0, 100], // show only values between 0 and 100
  },
  yaxis: {
    range: [-10, 10],
  },
};
```

This does NOT prevent zooming — it only defines the initial view.

## Preventing Zoom & Pan (Locking Bounds)

To enforce hard bounds so users cannot change them:

```js
layout = {
  xaxis: {
    range: [0, 100],
    fixedrange: true, // disables zoom & pan on this axis
  },
};
```

**Key behavior:**

- `fixedrange: true` → disables scroll zoom, drag zoom, and pan
- Works per-axis (you can lock only x or only y)

## Partial Locking (One Axis Only)

Example: Lock X axis but allow Y zoom

```js
layout = {
  xaxis: { range: [0, 100], fixedrange: true },
  yaxis: { fixedrange: false },
};
```

Useful for time-series charts where horizontal navigation must remain fixed.

## Autorange vs Range

### Autorange

```js
xaxis: {
  autorange: true;
}
```

Plotly calculates bounds automatically from data.

### Range Overrides Autorange

If `range` is provided, `autorange` becomes false implicitly.

## Dynamic Bounds Update

Bounds can be changed programmatically:

```js
Plotly.relayout(graphDiv, {
  "xaxis.range": [20, 50],
});
```

This updates the viewport without re-rendering the entire chart.

## Important Notes

### 1. Bounds vs Data Limits

Bounds only affect the visible window — data outside the range still exists.

### 2. Interaction Modes Still Matter

Even with bounds set:

- Users can zoom unless `fixedrange` is enabled
- Drag modes like zoom, pan, or drawing tools may modify view

### 3. Shapes and Drawing

If drawing tools are enabled, users can still draw outside visible bounds, but shapes may be clipped from view.

### 4. Interaction Overrides

You can disable all drag-based interactions (zoom, pan, select):

```js
layout = {
  dragmode: false, // disables all default zoom/pan tools
};
```

---

# Plotly Axis Labels Capability

Axis labels describe what each axis represents (e.g., Time, Price, Temperature). They help users interpret the data correctly.

In Plotly, axis labels are controlled via the axis `title` property.

## Setting Axis Labels

Basic example:

```js
layout = {
  xaxis: {
    title: { text: "Time (seconds)" },
  },
  yaxis: {
    title: { text: "Amplitude" },
  },
};
```

## Styling Axis Labels

You can customize font, size, and color:

```js
xaxis: {
  title: {
    text: 'Distance',
    font: {
      family: 'Arial',
      size: 16,
      color: '#ffffff'
    }
  }
}
```

## Title Position & Spacing

Control distance from axis using `standoff` and prevent clipping with `automargin`:

```js
xaxis: {
  title: { text: 'Distance', standoff: 20 },
  automargin: true
}
```

- `standoff` → space between axis and title
- `automargin` → expands margins automatically so labels fit

## Tick Label Formatting

Format numbers, dates, or categories:

```js
xaxis: {
  tickformat: ".2f"; // two decimal places
}
```

Examples:

- `.2f` → fixed decimals
- `%` → percentage
- Date formats for time axes

## Multi‑Line Labels

Axis titles support line breaks using `<br>`:

```js
xaxis: {
  title: {
    text: "Revenue<br>(in USD)";
  }
}
```

## Label Positioning via Margins

Plotly positions titles automatically, but spacing can be adjusted:

```js
layout = {
  margin: { l: 80, r: 40, t: 40, b: 80 },
};
```

Larger margins prevent labels from being cut off.

## Rotating Tick Labels (Not Axis Title)

Tick labels (numbers/categories) can be rotated for readability:

```js
xaxis: {
  tickangle: -45;
}
```

Useful for long category names.

## Hiding Axis Labels

To hide the axis title:

```js
xaxis: {
  title: {
    text: "";
  }
}
```

To hide tick labels entirely:

```js
xaxis: {
  showticklabels: false;
}
```

## Dynamic Updates

Axis labels can be changed without full redraw:

```js
Plotly.relayout(graphDiv, {
  "xaxis.title.text": "Updated Label",
});
```

---

# Plotly Legends Capability

A legend explains what each trace (line, bar, marker, etc.) represents in the chart. It maps visual styles (color, symbol, line type) to data series names.

In Plotly, legends are automatically generated from trace `name` properties.

## Basic Legend Usage

Each trace with a name appears in the legend:

```js
const data = [
  { x: [1, 2, 3], y: [2, 4, 6], type: "scatter", name: "Series A" },
  { x: [1, 2, 3], y: [1, 3, 5], type: "scatter", name: "Series B" },
];
```

## Show or Hide Legend

Control legend visibility from layout:

```js
layout = {
  showlegend: true,
};
```

Hide completely:

```js
layout = {
  showlegend: false,
};
```

## Legend Positioning

Place legend anywhere using normalized coordinates (0 → 1):

```js
layout = {
  legend: {
    x: 1, // right side
    y: 1, // top
  },
};
```

Common placements:

- Top‑right: `{ x: 1, y: 1 }`
- Bottom‑right: `{ x: 1, y: 0 }`
- Outside right: `{ x: 1.02, y: 1 }`
- Inside Top-Left: `{ x: 0.1, y: 1.1 }` (Relative to graph area)

## Legend Orientation

Horizontal legend (useful for dashboards):

```js
legend: {
  orientation: "h";
}
```

Default is vertical.

## Styling the Legend

Customize appearance:

```js
legend: {
  bgcolor: '#222',
  bordercolor: '#fff',
  borderwidth: 1,
  font: {
    family: 'Arial',
    size: 12,
    color: '#ffffff'
  }
}
```

## Legend Title

Add a title above legend items:

```js
legend: {
  title: {
    text: "Data Series";
  }
}
```

## Interaction Behavior

By default, clicking legend items:

- Single click → toggles trace visibility
- Double click → isolates one trace

Disable interactions:

```js
layout = {
  legend: { itemclick: false, itemdoubleclick: false },
};
```

## Grouped Legends

Group related traces together:

```js
{
  name: 'Temperature',
  legendgroup: 'weather'
}
```

Traces in the same group toggle together.

## Dynamic Updates

Legend settings can be changed programmatically:

```js
Plotly.relayout(graphDiv, {
  "legend.orientation": "h",
});
```

---

# Plotly Trace Modes Capability

Trace modes determine how data points and lines are visually represented. Modes can be combined using the `+` operator.

## Available Modes

### 1. Markers Only

Displays data points as individual dots.

```js
{
  mode: 'markers',
  marker: { size: 12 }
}
```

### 2. Lines Only

Connects data points with straight lines (no visible markers).

```js
{
  mode: "lines";
}
```

### 3. Lines + Markers

Displays both connecting lines and individual data points.

```js
{
  mode: "lines+markers";
}
```

### 4. Lines + Text

Displays connecting lines along with text labels at each point.

```js
{
  mode: 'lines+text',
  text: ['Pt 1', 'Pt 2', 'Pt 3', 'Pt 4']
}
```

---

# Plotly Shapes and Drawing Capability

Shapes are SVG elements (lines, rectangles, circles) that can be pre-loaded or drawn interactively.

## Pre-defined Shapes

Shapes are defined in the layout's `shapes` array:

```js
layout = {
  shapes: [
    { type: "line", x0: 0, y0: 8, x1: 10, y1: 8, line: { color: "red" } },
    {
      type: "rect",
      x0: 2,
      y0: 0,
      x1: 4,
      y1: 5,
      fillcolor: "green",
      opacity: 0.2,
    },
    { type: "circle", x0: 6, y0: 4, x1: 7, y1: 7, line: { color: "blue" } },
  ],
};
```

## Interactive Drawing

Enable drawing tools via `dragmode` and the Modebar:

```js
const layout = {
  dragmode: "drawline", // Default interaction mode
};

const config = {
  modeBarButtonsToAdd: ["drawline", "eraseshape", "drawrect", "drawcircle"],
};
```

---

# Plotly Dynamic Graph Modification

Plotly provides efficient methods to modify existing graphs without a full re-render.

| Method                | Description                          | Example                                             |
| --------------------- | ------------------------------------ | --------------------------------------------------- |
| `Plotly.restyle`      | Update data or trace properties      | `Plotly.restyle(div, { 'line.color': 'red' }, [0])` |
| `Plotly.relayout`     | Update layout (titles, axes, legend) | `Plotly.relayout(div, { title: 'New Title' })`      |
| `Plotly.addTraces`    | Append new data series               | `Plotly.addTraces(div, { x: [1,2], y: [3,4] })`     |
| `Plotly.deleteTraces` | Remove traces by index               | `Plotly.deleteTraces(div, -1)` (removes last)       |

---

# Plotly Event Listeners

Capture user interactions to trigger custom application logic.

### 1. Click Events

Triggered when a user clicks a data point.

```js
graphDiv.on("plotly_click", function (data) {
  const pt = data.points[0];
  console.log(`Clicked x=${pt.x}, y=${pt.y}`);
});
```

### 2. Hover & Unhover

Triggered when the mouse enters or leaves a point's vicinity.

```js
graphDiv.on("plotly_hover", function (data) {
  // Update custom tooltip or UI
});

graphDiv.on("plotly_unhover", function (data) {
  // Reset state
});
```

### 3. Relayout (Zoom/Pan)

Triggered whenever the layout changes, including zoom and pan operations.

```js
graphDiv.on("plotly_relayout", function (eventData) {
  if (eventData["xaxis.range[0]"]) {
    console.log("New X-Range:", eventData["xaxis.range[0]"]);
  }
});
```

---

# Plotly.js Modules Overview (JS/TS)

Plotly provides various JS bundles to balance between feature set and bundle size.

## Available Bundles

### 1. Full Bundle

- **Module**: `plotly.js-dist`
- **Bundle Size**: 3.5 – 5 MB
- **Supported Charts**: All (scatter, line, bar, pie, 3D, maps, financial, tables, etc.)
- **Pros**: Everything included, works out-of-the-box.
- **Cons**: Very heavy, slower initial load.

### 2. Basic Bundle

- **Module**: `plotly.js-basic-dist`
- **Bundle Size**: ~600 KB
- **Supported Charts**: Scatter, line, bar, pie.
- **Pros**: Lightweight, fast load.
- **Cons**: Missing advanced charts (3D, maps, financial).

### 3. Cartesian Bundle

- **Module**: `plotly.js-cartesian-dist`
- **Bundle Size**: ~1 MB
- **Supported Charts**: 2D only (scatter, line, bar, area).
- **Pros**: Smaller than full, optimized for 2D dashboards.
- **Cons**: No 3D, maps, or ternary plots.

### 4. 2D WebGL Bundle

- **Module**: `plotly.js-gl2d-dist`
- **Bundle Size**: > 1 MB
- **Supported Charts**: `scattergl`, `heatmapgl`, large 2D datasets.
- **Pros**: WebGL acceleration for >100k points, smooth pan/zoom.
- **Cons**: Only WebGL-supported trace types; not all annotations/shapes supported.

### 5. 3D WebGL Bundle

- **Module**: `plotly.js-gl3d-dist`
- **Bundle Size**: > 1 MB
- **Supported Charts**: `scatter3d`, `surface`, `mesh3d`.
- **Pros**: Full 3D visualization, interactive rotation.
- **Cons**: Slower on older devices, requires WebGL.

### 6. Geo Bundle

- **Module**: `plotly.js-geo-dist`
- **Bundle Size**: ~700 KB
- **Supported Charts**: Maps, choropleths.
- **Pros**: Specialized for geographic data.
- **Cons**: No Cartesian or 3D charts; needs TopoJSON for custom maps.

## Recommendation Summary

| Bundle        | Module Name                | Key Included Charts                          | Missing Features               |
| ------------- | -------------------------- | -------------------------------------------- | ------------------------------ |
| **Basic**     | `plotly.js-basic-dist`     | Scatter, Bar, Pie                            | No 3D, Maps, Finance, Heatmaps |
| **Cartesian** | `plotly.js-cartesian-dist` | 2D SVG: Bar, Box, Heatmap, Histogram, Violin | No 3D, Maps, WebGL             |
| **GL2D**      | `plotly.js-gl2d-dist`      | Scattergl, Splom, Parcoords                  | No 3D, Maps, SVG-only 2D       |
| **GL3D**      | `plotly.js-gl3d-dist`      | Scatter3d, Surface, Mesh3d, Cone             | No 2D, Maps                    |
| **Geo**       | `plotly.js-geo-dist`       | Choropleth, Scattergeo                       | No 2D/3D charts                |

## Plotly @Types Bundle

- Recent versions of the main `plotly.js` package include **built-in type definitions**.
- The `-dist` packages (e.g., `plotly.js-basic-dist`) often **do not** ship with types.
- To get types for a partial bundle, point your `tsconfig.json` paths to the main `plotly.js` types or use a shim.

## Custom Bundling

For maximum optimization, you can import only the specific modules you need.

```js
// Example of custom bundling (reduces size significantly)
import Plotly from "plotly.js/lib/core";
import bar from "plotly.js/lib/bar";
import pie from "plotly.js/lib/pie";

// Register the partial modules
Plotly.register([bar, pie]);

export default Plotly;
```

---

# Plotly.js Vue/Nuxt Integration (JS/TS)

This section covers integration-specific approaches for Vue and Nuxt projects.

## Integration Approaches

### 1. Your Own Wrapper (Custom Component)

- **Description**: Create a custom Vue/Nuxt component importing the specific Plotly bundle needed.
- **Pros**: Full control over events, shapes, and interactivity; optimized performance; full TypeScript support.
- **Cons**: Manual setup; must handle client-only rendering in SSR manually.
- **Use Case**: Advanced dashboards, dynamic layouts, large datasets.

> [!IMPORTANT]
> **Avoid Reactive Plotly Instances:** Do not store the Plotly chart object in a `ref()` or `reactive()` state. Vue's Proxy system will track every internal property of the Plotly instance, leading to extreme lag or browser crashes. Use `shallowRef()` or store it as a non-reactive property.

### 2. Vue Wrapper

- **Module**: `vue-plotly` (Vue 2) / `vue-plotly-next` (Vue 3)
- **Description**: Allows using Plotly charts as declarative Vue components.
- **Pros**: Fully declarative; supports standard Plotly events.
- **Cons**: Requires explicit Plotly bundle import; WebGL sizing requires care.
- **Use Case**: Standard Vue apps with moderate interactivity.

### 3. Nuxt Module

- **Module**: `nuxt-plotly` (unofficial)
- **Description**: Module for easy Plotly integration in SSR/SPA apps.
- **Pros**: SSR-friendly; handles dynamic imports and `<ClientOnly>` automatically.
- **Cons**: Unofficial; limited documentation for advanced custom events; often imports the heavy full bundle.
- **Use Case**: Quick Nuxt integration for basic charts.

## Decision Table

| Requirement                  | Recommended Approach | Reason                                                             |
| ---------------------------- | -------------------- | ------------------------------------------------------------------ |
| **Performance (Large Data)** | Your Own Wrapper     | Avoids Vue Proxy overhead; allows `shallowRef`.                    |
| **Nuxt / SSR**               | Your Own Wrapper     | Using `<ClientOnly>` with `onMounted` is more stable than modules. |
| **Rapid Prototyping**        | `vue-plotly-next`    | Quickest way to get a chart on screen with declarative props.      |
| **Strict TypeScript**        | Your Own Wrapper     | Direct access to `Plotly.Data` and `Plotly.Layout` types.          |

## Key Notes

- All wrappers require an underlying Plotly bundle (`-dist` packages).
- Custom wrappers provide the best performance tuning and TS support.
- Events, shapes, and WebGL features are fully supported if the underlying bundle supports them.
