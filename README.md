# 回路/Kairo/Pulse Simulation

A small HTML5 canvas experiment inspired by the computer program shown in  *Kairo* (回路, 2001) by Kiyoshi Kurosawa.

"That? Something we programmed here.
If two dots get too close, they die,
but if they get too far apart, they're drawn closer."

"A miniature model of our world..."

**As said in the movie:** "I wouldn't suggest staring at it for too long..."

---

## Getting Started

1. Save the HTML in a folder:

   ```text
   project/
   └── simulation.html
   ```

2. Open `simulation.html` in any modern browser (Chrome, Firefox, Edge, etc.).

That’s it. No build step, no dependencies.

---

## How It Works

The simulation is a basic particle system:

- Each **dot** has:
  - `x, y` – position  
  - `vx, vy` – velocity  
  - `radius` – visual size  
  - `alpha` – brightness  

- Each frame:
  1. For every dot, find its **nearest neighbour**.
  2. Compute the **edge-to-edge gap** between the two dots:
     ```js
     const dist = Math.sqrt(dx*dx + dy*dy);
     const gap = dist - (a.radius + b.radius);
     ```
  3. If `gap < MIN_GAP` -> both dots die and are respawned.  
  4. If `gap > MAX_GAP` -> apply a weak attraction force pulling them together.  
  5. Update positions, wrap around the screen edges, then redraw.

---

## Tuning the Behaviour

At the top of the script you’ll find the main parameters:

```js
const NUM_DOTS        = 300;

// proximity based on edge-to-edge gap
const MIN_GAP          = 0;      // if edges touch/overlap they die
const MAX_GAP          = 80;     // if edges farther apart they attract
const ATTRACTION       = 0.0008; // strength of attraction
const MAX_SPEED        = 0.6;    // velocity cap
const RESPAWN_ON_DEATH = false;   // keep population constant
```

*Note:* I keep the respawn as false as I think it's more film accurate.

### Visual Style

Inside the `Dot` class:

```js
this.radius = 1 + Math.random() * 8;     // dot size range
this.alpha  = 0.2 + Math.random() * 0.8; // brightness
```

Increase `radius` for bigger dots or tweak `alpha` for dimmer/brighter ones.
You can also adjust `shadowBlur` in `draw()` to make the glow softer or sharper:

```js
ctx.shadowBlur = this.radius * 3;
```

---

## License

MIT – feel free to fork, remix, and use in your own projects.
