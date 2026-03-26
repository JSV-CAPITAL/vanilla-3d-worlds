# vanilla-3d-worlds

A Claude Code skill for creating beautiful, immersive 3D worlds and animations using vanilla JavaScript with Three.js, raw WebGL, or WebGPU.

## What it does

This skill guides Claude through a structured workflow for building browser-based 3D experiences:

1. **Discovery** — Asks the right questions one at a time to understand what you're envisioning (scene type, aesthetic, camera, interaction, mood, platform)
2. **Design philosophy** — Writes a `DESIGN.md` capturing the creative direction before any code
3. **Implementation** — Builds a modular project with proper lighting, materials, post-processing, animation, and performance optimization
4. **Polish** — Quality pass for visual impact, responsiveness, and edge cases

## What it's great at

| Scene Type | Quality | Why |
|---|---|---|
| Particle systems (galaxies, nebulas, starfields) | Excellent | Pure math — shaders + bloom = stunning |
| Ocean / water | Excellent | Gerstner waves are inherently procedural |
| Terrain / landscapes | Excellent | Noise displacement + height coloring + fog |
| Abstract geometry (hero sections, data viz) | Excellent | Floating shapes, mouse parallax, clean aesthetics |
| Shader effects (portals, dissolves, raymarching) | Excellent | GLSL is where code shines |
| Product showcases (with .glb models) | Great | Skill builds the scene; you provide the model |

## What it's honest about

Procedural code can't replicate everything. The skill will steer you toward what works and away from what doesn't:

- Cities/architecture from code alone — boxes with textures don't look like buildings
- Organic products without a real 3D model — code can't sculpt a recognizable shoe
- Characters/creatures — way beyond procedural geometry

When you ask for these, the skill suggests either providing art assets or taking an abstract approach that plays to code's strengths.

## Install

```bash
claude install-skill https://github.com/JSV-CAPITAL/vanilla-3d-worlds
```

## Includes

- **SKILL.md** — Full skill with discovery workflow, implementation guidance, performance hard limits, lighting tables, and common recipes
- **references/shader-patterns.md** — Ready-to-use GLSL snippets: noise (simplex, FBM), vertex displacement, fog, Fresnel glow, Gerstner waves, color grading, particle shaders
- **evals/evals.json** — Test cases for validation

## Tech covered

- **Three.js** — Default recommendation. Rich ecosystem, post-processing, loaders, controls.
- **Raw WebGL** — For full control, learning, or minimal footprint.
- **WebGPU** — For compute-heavy scenes (massive particles, fluid sim). Bleeding edge.

## Performance guardrails

The skill enforces real-world performance limits:

- Max 5-8 dynamic lights (use emissive materials + bloom to fake more)
- Keep draw calls under 200 (use `InstancedMesh` for repeated objects)
- Cap pixel ratio at 2
- Frame-rate independent animation via delta time
- Pause render loop on tab visibility change

## Example prompts

> "I want a 3D particle galaxy for my portfolio hero section — cosmic, calm, mouse-reactive"

> "Build me a terrain flyover with mountains, fog, and touch controls for mobile"

> "I need a product showcase with dramatic lighting for a sneaker — I have the .glb file"

> "Make an ocean scene with Gerstner waves and a sunset sky"

## License

MIT
