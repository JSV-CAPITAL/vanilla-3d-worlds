---
name: vanilla-3d-worlds
description: >
  Create beautiful, immersive 3D worlds, scenes, and animations using vanilla JavaScript with Three.js, raw WebGL, or WebGPU.
  Use this skill whenever the user wants to build 3D environments, interactive scenes, particle systems, shader effects,
  3D landing page heroes, game-like worlds, physics simulations, generative 3D art, or any kind of browser-based 3D experience.
  Also trigger when the user mentions "Three.js," "WebGL," "WebGPU," "3D scene," "3D world," "3D animation," "shader,"
  "GLSL," "particle system," "raymarching," "procedural terrain," "3D visualization," "orbit controls," "skybox,"
  "PBR materials," "post-processing," or wants to create anything that renders in 3D in the browser.
  Even if they just say "make something cool in 3D" or "I want an interactive 3D thing" — use this skill.
---

# Vanilla 3D Worlds

Create breathtaking 3D experiences in the browser using vanilla JavaScript. This skill covers Three.js, raw WebGL, and WebGPU — from serene landscapes to cyberpunk cities, from particle galaxies to architectural walkthroughs.

## The Cardinal Rule

**Never start building until you deeply understand the user's vision.**

3D is where the gap between imagination and output is widest. A user who says "make a 3D forest" might picture a photorealistic Unreal-quality scene, a low-poly stylized world, a wireframe abstract interpretation, or a single tree with magical particles. The discovery phase exists to close that gap. Skip it, and you'll build the wrong thing.

---

## Phase 1: Discovery — Understanding the Vision

Before writing any code, have a focused conversation to understand what the user sees in their mind. Ask questions **one at a time**. Prefer **multiple choice** when it helps narrow options faster, but use open-ended questions when the user needs room to describe something specific.

### Discovery Sequence

Work through these areas in order. You don't need to ask every question — skip what's already clear from context. But don't skip the *areas*. Each one matters.

**1. The Scene — What are we building?**

Start broad: what is the world?

> What kind of 3D experience are you imagining?
> - A) An environment to explore (landscape, city, space, interior)
> - B) An animated visualization (particles, data, abstract geometry)
> - C) A hero section / landing page background
> - D) A game-like interactive world
> - E) Something else — describe it

Follow up based on their answer. Get specific: what's in the scene? What's the setting? Is there a narrative or purpose?

**2. The Aesthetic — How should it look?**

This is where most 3D projects succeed or fail. Push for specificity.

> What visual style fits your vision?
> - A) Photorealistic — physically accurate lighting, materials, reflections
> - B) Stylized / low-poly — geometric, colorful, playful
> - C) Cyberpunk / neon — dark environments, glowing edges, bloom effects
> - D) Organic / natural — soft lighting, atmospheric fog, earthy tones
> - E) Abstract / minimal — clean geometry, monochrome or limited palette
> - F) Retro / synthwave — grid floors, sunsets, 80s sci-fi
> - G) Something specific — describe it or share a reference

If they share a reference image, screenshot, or URL — study it carefully. Identify: color palette, lighting direction, material types, level of detail, mood.

**3. The Camera — How do we see the world?**

> How should the user view and navigate the scene?
> - A) Orbit controls — rotate around a central object (product showcase, sculpture)
> - B) First-person — WASD/arrow movement through the world
> - C) Fly-through — automatic cinematic camera path
> - D) Fixed camera with parallax — subtle movement on mouse/scroll
> - E) Multiple viewpoints — switch between cameras
> - F) Not sure — recommend based on the scene

**4. Interaction — What can the user do?**

> What level of interactivity do you want?
> - A) Passive / cinematic — it plays, the user watches
> - B) Mouse-reactive — scene responds to cursor position or clicks
> - C) Fully interactive — user can move, click objects, trigger events
> - D) Scroll-driven — scene animates as user scrolls the page
> - E) Touch-friendly — needs to work well on mobile

**5. The Mood — How should it feel?**

This question matters more than people realize. The same geometry with different lighting and post-processing can feel serene or menacing.

> What emotional tone are you going for?
> - A) Calm / meditative — slow movement, soft light, ambient sound
> - B) Energetic / dynamic — fast particles, bright colors, motion
> - C) Mysterious / dark — shadows, fog, subtle reveals
> - D) Playful / whimsical — bouncy physics, bright palette, surprises
> - E) Epic / grand — vast scale, dramatic lighting, cinematic
> - F) Something else — describe the feeling

**6. Performance & Platform**

> Any constraints I should know about?
> - Target devices (desktop only, mobile too, VR?)
> - Expected complexity (how many objects, particles, etc.)
> - Does this need to run alongside a heavy web app or standalone?
> - Any preference between Three.js, raw WebGL, or WebGPU?

If the user doesn't have a preference on technology, recommend based on the project:
- **Three.js**: Best default. Rich ecosystem, post-processing, loaders, controls. Use for most projects.
- **Raw WebGL**: When the user wants full control, is learning graphics programming, or needs a minimal footprint.
- **WebGPU**: For compute-heavy scenes (massive particle systems, GPU-driven simulation). Note: browser support is still expanding.

### Discovery Principles

- **One question per message.** Don't fire off a questionnaire.
- **React to their answers.** If they say "cyberpunk city," your next question should build on that — not robotically move to the next category.
- **Offer visual references when possible.** Describe specific visual examples: "Think Blade Runner 2049 neon-in-fog, or more Tron Legacy clean-grid?"
- **Read between the lines.** "Something cool" usually means visually impressive and interactive. "Professional" usually means subtle, performant, and polished. "Fun" usually means particles and physics.
- **If they have a reference, that's gold.** A single screenshot or URL can answer most discovery questions at once. Ask what they like about it and what they'd change.

---

## Phase 2: Design Philosophy

Once discovery is complete, write a short design philosophy (3-5 paragraphs) before any code. This captures the creative direction and becomes the north star for implementation.

The philosophy should cover:
- **The concept**: What is this world and why does it exist?
- **The visual language**: What defines the look? (lighting, palette, materials, detail level)
- **The feeling**: What should someone experience when they see it?
- **The movement**: How does motion reinforce the mood? (camera, objects, particles)

Write this in a `DESIGN.md` file at the project root. It keeps you honest — when you're deep in shader code, it's easy to lose sight of the original vision.

---

## Phase 3: Implementation

### Project Structure

Output a proper project structure, not a single HTML file. This keeps things maintainable and lets the user extend later.

```
project-name/
  index.html              # Entry point, canvas setup
  style.css               # Full-bleed canvas + any UI overlay styles
  src/
    main.js               # Scene initialization, render loop, resize handling
    scene.js              # Scene graph: objects, groups, hierarchy
    camera.js             # Camera setup and controls
    lighting.js           # Lights, shadows, environment maps
    materials.js          # Material definitions, textures, shaders
    animation.js          # Animation loops, tweens, transitions
    interaction.js        # Mouse, keyboard, touch, scroll handlers
    postprocessing.js     # Bloom, DOF, color grading, etc. (if needed)
    utils.js              # Helpers: math, color, random, loaders
  shaders/                # Custom GLSL (if needed)
    vertex.glsl
    fragment.glsl
  assets/                 # Textures, models, HDRIs, audio
  DESIGN.md               # Design philosophy document
```

Adapt this structure to the project's complexity — a simple particle animation doesn't need 10 files. But always separate concerns enough that each file has one clear purpose.

### The Render Foundation

Every project starts with a solid foundation. Get these right first:

**Canvas setup:**
- Full-viewport canvas with proper CSS (`width: 100vw; height: 100vh; display: block`)
- Handle resize events — update camera aspect ratio and renderer size
- Use `devicePixelRatio` but cap at 2 for performance
- Set up the animation loop with `requestAnimationFrame`
- Use `THREE.Clock` or `performance.now()` for frame-rate-independent animation

**Renderer configuration:**
- `antialias: true` for smooth edges (disable on mobile if needed)
- `alpha: true` if the scene composites over page content
- `toneMapping` — use `ACESFilmicToneMapping` for cinematic look, `LinearToneMapping` for accuracy
- `outputColorSpace = THREE.SRGBColorSpace` — correct color space for web
- Enable `shadowMap` if using shadows; `PCFSoftShadowMap` for quality

**Camera setup:**
- Perspective camera with appropriate FOV (50-75 for scenes, 35-50 for architectural, 90+ for VR)
- Set near/far planes tight to the scene to maximize depth buffer precision
- Position camera to frame the scene well on initial load

### Lighting That Makes or Breaks the Scene

Lighting is the single biggest factor in visual quality. Approach it like a cinematographer.

**Key principles:**
- **Three-point lighting as baseline**: key light (main directional), fill light (soft ambient), rim/back light (accent)
- **Environment maps**: Use HDR environment maps for realistic reflections. A good HDRI does more for realism than any material setting.
- **Shadows**: Soft shadows sell realism. Use `PCFSoftShadowMap`, tune `shadow.mapSize` (1024-2048), and set tight shadow camera bounds.
- **Color temperature**: Mix warm and cool lights. Pure white light looks flat and artificial.
- **Less is more**: One well-placed light beats five mediocre ones.

**By aesthetic:**
| Style | Key Light | Fill | Accent | Ambient |
|-------|-----------|------|--------|---------|
| Photorealistic | Directional (sun) | Hemisphere | Point lights | HDRI environment |
| Cyberpunk | Minimal | None | Neon point/spot lights, emissive materials | Dark ambient (0x111122) |
| Organic | Warm directional | Hemisphere (sky/ground) | Volumetric fog | Soft ambient |
| Minimal | Single directional | Very subtle | None | Clean white/gray |
| Synthwave | Low directional | None | Grid glow, emissive | Dark purple ambient |

### Materials & Textures

- **PBR (MeshStandardMaterial/MeshPhysicalMaterial)** for realistic surfaces — set `roughness`, `metalness`, and use texture maps
- **MeshToonMaterial** for stylized/cel-shaded looks
- **ShaderMaterial/RawShaderMaterial** for custom effects — write GLSL when built-in materials can't achieve the look
- **Emissive materials** for self-illuminating elements (screens, neon, magic effects)
- For custom shaders, keep vertex and fragment shaders in separate `.glsl` files and load them

### Placeholder & Fallback Geometry

When a project expects a loaded model (.glb, .gltf, .obj) but needs to work without it (demo mode, loading failure):

**Best approach: Use an abstract placeholder, not a bad approximation.** Procedural geometry cannot convincingly replicate organic products like shoes, headphones, or bottles. A sleek abstract shape (rounded box, torus knot, icosahedron) with the right materials and lighting looks intentional and premium. A bad shoe shape looks broken.

**When to attempt a recognizable shape:** Only for simple, geometric products — a phone (box with rounded corners), a bottle (lathe geometry), a ring (torus). If the real product has organic curves, go abstract.

**Do:**
- Use a visually striking geometric primitive as the focal object
- Apply the same PBR materials, accent colors, and lighting the real model would get
- Add a subtle text overlay or loading hint: "Drop your .glb into assets/ to see your model"
- Keep the scene fully functional — all controls, animation, particles should work with the placeholder

**Don't:**
- Stack untransformed boxes — this always looks terrible
- Write vertex rounding/taper helpers that don't actually write back to the buffer (common bug: computing new positions but never calling `pos.setXYZ()`)
- Skip the fallback entirely — a spinning empty scene is worse than a simple shape

### Post-Processing

Post-processing transforms a good scene into a cinematic one. Use sparingly — each pass costs performance.

**Essential passes by mood:**
- **Bloom** (`UnrealBloomPass`): Neon, magic, sci-fi. Makes bright things glow.
- **Depth of Field** (`BokehPass`): Focus attention, add depth. Great for product shots.
- **Color grading** (`LUTPass` or custom): Unify the color palette. Warm for organic, cool for sci-fi.
- **Film grain** (custom shader): Adds texture and removes the "too clean CG" look.
- **Vignette**: Subtle darkening at edges, draws focus to center.
- **SSAO** (`SSAOPass`): Ambient occlusion in screen space — adds depth to crevices.
- **Fog**: Not technically post-processing, but crucial for atmosphere and hiding draw distance.

### Animation Principles

- **Ease everything.** Linear motion looks mechanical. Use easing for organic feel.
- **Stagger groups.** When multiple objects animate, offset their timing slightly.
- **Breathe.** Add subtle constant motion — gentle sway, floating, pulsing glow. Static 3D scenes feel dead.
- **Respond to input.** Even passive scenes benefit from subtle mouse parallax.
- **Use delta time.** Always multiply movement by `deltaTime` from `Clock.getDelta()` so animation speed is independent of frame rate.
- For complex sequenced animations, reference the GSAP skills (gsap-core, gsap-timeline) — GSAP works great with Three.js.

### Performance

Performance isn't an afterthought — it determines whether the experience is "beautiful" or "beautiful but unusable."

**Geometry:**
- Use `BufferGeometry` (never legacy `Geometry`)
- `InstancedMesh` for repeated objects (trees, particles, buildings)
- Merge static geometries with `BufferGeometryUtils.mergeGeometries()`
- LOD (Level of Detail) for large scenes — swap geometry based on camera distance

**Materials:**
- Reuse materials across meshes that share the same look
- Minimize unique shader programs (each unique material = potential shader compile)
- Texture atlases for many small textures

**Rendering:**
- Frustum culling is on by default — make sure bounding spheres are correct
- Use `renderer.info` to monitor draw calls, triangles, textures
- Cap pixel ratio: `Math.min(window.devicePixelRatio, 2)`
- For heavy scenes, render at lower resolution and upscale

**Memory:**
- Dispose geometries, materials, and textures when removing objects
- Dispose render targets and post-processing passes on cleanup
- Watch for texture memory — compress and resize appropriately

**Hard limits for real-time scenes:**
- **Dynamic lights**: Max 5-8 total (point + spot). Every dynamic light multiplies draw calls. Use emissive materials and bloom to fake the appearance of more lights — a neon sign with `MeshBasicMaterial` and bloom looks like it's casting light without costing a light slot.
- **Draw calls**: Keep under 200. Use `InstancedMesh` for repeated objects (buildings, trees, particles). 280 individual meshes with unique textures is a performance disaster.
- **Particles**: 5,000-10,000 is comfortable. Above 10,000, use InstancedMesh or compute shaders instead of Points.
- **Canvas textures**: Generate a single texture atlas, not one per object. 280 unique canvas textures means 280 GPU texture uploads.

**Targets:**
- 60fps on desktop, 30fps acceptable on mobile
- First meaningful render under 2 seconds
- Total JS bundle under 500KB (Three.js core is ~150KB gzipped)

### Raw WebGL Path

When the user wants raw WebGL (learning, minimal footprint, full control):

- Set up the WebGL context, viewport, and clear color manually
- Write vertex and fragment shaders as strings or load from `.glsl` files
- Compile shaders, link programs, set up attribute buffers and uniforms
- Implement your own render loop with `requestAnimationFrame`
- Build minimal abstractions as needed — a `createProgram()` helper, a `createBuffer()` helper
- Don't recreate Three.js — keep it lean and purposeful

### WebGPU Path

When targeting WebGPU for compute-heavy work:

- Check for `navigator.gpu` support and provide a WebGL fallback
- Use compute shaders for massive particle systems, fluid simulation, terrain generation
- Leverage storage buffers for GPU-side state
- Pipeline caching matters — create pipelines once, reuse
- This is bleeding edge — document browser requirements clearly

---

## Phase 4: Polish & Refinement

The code can be perfectly structured and still look terrible. Visual quality is what the user actually judges. Before delivering, do this quality pass:

1. **First impression**: Does the initial view land? The camera position, lighting, and first frame should be striking. If you see it and don't think "wow," the user won't either.
2. **Geometry check**: Do all shapes look intentional? Untransformed boxes, broken rounding functions, or geometry that doesn't resemble what it's supposed to be will ruin an otherwise polished scene. If a function modifies vertex positions, verify it actually writes back to the buffer attribute with `setXYZ()`.
3. **Loading experience**: Add a loading screen or progress bar for scenes with assets. Nobody should stare at a black canvas.
4. **Responsive**: Resize the browser — does it adapt cleanly? Test portrait and landscape.
5. **Performance**: Open DevTools, check frame rate. If below 60fps, identify the bottleneck (GPU? CPU? memory?).
6. **The 10-second test**: Show it to someone for 10 seconds. If they don't react, the visual impact isn't strong enough.
7. **Motion feels right**: Do animations feel natural? Too fast feels frantic, too slow feels broken.
8. **Edge cases**: What happens on page visibility change? On tab switch? Pause the render loop when hidden.

---

## What Works Well vs. What Doesn't

This skill generates code — not art assets. Some 3D scenes are naturally mathematical and look stunning when generated procedurally. Others require models, textures, and art direction that code alone can't provide. Know the difference before committing to a direction.

**Scenes that look great procedurally (play to your strengths):**
- Particle systems: galaxies, nebulas, fireflies, snow, starfields
- Water/ocean: Gerstner waves are pure math and look genuinely beautiful
- Terrain/landscapes: noise-based displacement with height coloring and fog
- Abstract geometry: floating shapes, data visualizations, generative art
- Shader effects: portals, dissolves, distortion, raymarching
- Product showcases: when the user provides a real .glb model — the scene (lighting, controls, particles, post-processing) is what the skill builds

**Scenes that look bad procedurally (steer users away or set expectations):**
- Cities/architecture: boxes with noisy textures don't read as buildings. Procedural cities need real modeling tools, not BoxGeometry.
- Organic products without a model: shoes, headphones, furniture — code can't sculpt recognizable shapes
- Characters/creatures: way beyond procedural geometry
- Interior scenes: rooms full of furniture need models

When a user asks for something in the "looks bad" category, be upfront: "A cyberpunk city that looks like Blade Runner needs 3D models — I can build an amazing scene with the right assets, or I can create a stylized/abstract take that leans into what code does well (neon particle rain over a dark void, glowing grid lines receding into fog, etc.)"

## Quick Reference: Common Recipes

**Starfield / particle galaxy:**
Points geometry + custom shader material, size attenuation, subtle rotation, bloom post-processing.

**Terrain / landscape:**
Plane geometry with vertex displacement (noise-based), custom shader for height-based coloring, fog for atmosphere.

**Product showcase (with .glb model):**
Center-stage object, orbit controls, dramatic lighting, contact shadow, particles, bloom. The skill builds the scene — the user provides the model. For a fallback/demo, use a clean abstract geometric shape (torus knot, icosahedron) rather than attempting to replicate the product.

**Floating geometry (abstract):**
InstancedMesh, randomized positions/rotations, gentle sinusoidal animation, subtle mouse parallax.

**Ocean / water:**
Custom vertex shader with Gerstner waves (7+ layered waves for realism), Fresnel reflections, subsurface scattering, procedural sky with sun disc. This is one of the best use cases — waves are pure math and look stunning.

**Portal / vortex effect:**
Torus geometry + custom fragment shader with animated UV distortion, particle ring, bloom.

**Cyberpunk / sci-fi atmosphere (abstract approach):**
Instead of trying to build a city from boxes, create an atmospheric abstract scene: neon particle rain falling through fog, glowing grid lines on a dark plane receding to infinity, floating holographic panels, volumetric light beams. This looks far better than noisy textured boxes pretending to be buildings.

---

## What This Skill Does NOT Cover

- **React Three Fiber (R3F)**: This skill is vanilla JS. For React-based 3D, the user should use the React ecosystem.
- **Pre-built game engines**: No Unity, Unreal, Godot export-to-web workflows.
- **3D modeling**: This skill builds scenes in code. For importing existing models (.gltf, .obj, .fbx), we load and place them — we don't create the models themselves.
- **VR/AR (WebXR)**: Mention if relevant to the user's goals, but detailed WebXR implementation is beyond this skill's scope.
