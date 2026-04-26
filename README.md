# FrequenzyVisualizer
A modular audio-reactive canvas visualizer.

The goal of this project is to make it easy for people to add new creative pieces without needing to understand the entire codebase.

You can contribute by adding:

- a new **Behaviour**
- a new **Style / Shape**
- a new **Post FX effect**
- a new **Color Palette**

Each part is designed to be separate, but still work together as one visual system.

---

## How it works

The visualizer is built like a pipeline:

```txt
Audio Input
   ↓
Frequency Data
   ↓
Behaviour
   ↓
Style / Shape
   ↓
Post FX
   ↓
Color Palette
   ↓
Canvas Output
```

Each step has one clear responsibility.

| Module | What it does | Examples |
|---|---|---|
| Behaviour | Changes how the audio data moves or reacts | Rigid, Wobbly, Bouncy, Breathing |
| Style / Shape | Decides what gets drawn on the canvas | Bars, Circle, Wave, Orbit |
| Post FX | Modifies the finished canvas image | Bloom, VHS, Scanlines, Pixelate |
| Color Palette | Provides the colors used by the visualizer | Neon, Fire, Ocean, Synthwave |

The idea is that these parts can be mixed and matched.

A new behaviour should work with all styles.  
A new style should work with all behaviours.  
A new post effect should work on any style.  
A new color palette should work everywhere colors are used.

---

## Project philosophy

This project is meant to be friendly for hobby developers, creative coders, and people who just want to experiment with visuals.

You do not need to understand the whole project to contribute.

For example:

```txt
One person can add a new shape.
Another person can add a new behaviour.
Someone else can add a color palette.
Another contributor can add a post-processing effect.
```

As long as each contribution follows the module rules, everything should still work together.

---

## Module rules

### Behaviours

Behaviours change the movement, intensity, or reaction of the audio data.

A behaviour should:

- receive audio levels
- modify those levels
- return the modified levels

A behaviour should not:

- draw directly to the canvas
- choose its own colors
- apply post-processing effects

Example behaviour structure:

```js
const myBehaviour = {
  id: "myBehaviour",
  name: "My Behaviour",
  description: "Short explanation of what the behaviour does.",

  apply(levels, frame, time, amount) {
    return levels.map(level => ({
      ...level,
      value: level.value
    }));
  }
};
```

---

### Styles / Shapes

Styles decide what is drawn.

A style can draw bars, circles, waves, particles, tunnels, shapes, or anything else based on the audio data.

A style should:

- receive processed audio levels
- draw something to the canvas
- use the shared color system

A style should not:

- create its own audio system
- create its own behaviour logic
- hard-code its own palette system

Example style structure:

```js
const myStyle = {
  id: "myStyle",
  name: "My Style",
  description: "Short explanation of what the style draws.",

  draw(ctx, canvas, levels, frame, time) {
    // Draw your visual here
  }
};
```

---

### Post FX

Post FX run after the style has already drawn to the canvas.

These effects modify the final image, not the raw audio data.

A post effect should:

- read from the existing canvas
- modify the final visual output
- work with any style

A post effect should not:

- depend on one specific style
- create its own audio analysis
- replace the main drawing system

Example post effect structure:

```js
const myEffect = {
  id: "myEffect",
  name: "My Effect",
  description: "Short explanation of the effect.",

  apply(ctx, canvas, frame, time) {
    // Modify the final canvas image here
  }
};
```

---

### Color Palettes

Palettes provide colors for the visualizer.

A palette should only define colors. It should not contain drawing logic, audio logic, or effect logic.

Example palette structure:

```js
const myPalette = {
  id: "myPalette",
  name: "My Palette",
  colors: [
    "#ff00cc",
    "#ff7a00",
    "#ffe600",
    "#00ffcc",
    "#6a5cff"
  ]
};
```

---

## Contribution checklist

Before submitting a contribution, try to test it with a few combinations.

If you add a behaviour, test it with multiple styles.  
If you add a style, test it with multiple behaviours and palettes.  
If you add a post effect, test it with several different styles.  
If you add a palette, test it with several styles and color modes.

The goal is not that every combination looks perfect.

The goal is that everything still works together.

---

## Design goal

The project should feel like a creative playground.

Each part should be simple enough to understand on its own, but powerful when combined with the other parts.

```txt
Behaviour + Style + Post FX + Palette = Final Visual
```

That is the core idea of the project.
