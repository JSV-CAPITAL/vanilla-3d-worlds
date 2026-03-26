# Common Shader Patterns

Reference for frequently-used GLSL patterns in 3D world building. Read this when implementing custom shaders.

## Table of Contents
1. [Noise Functions](#noise-functions)
2. [Vertex Displacement](#vertex-displacement)
3. [Fog & Atmosphere](#fog--atmosphere)
4. [Glow & Emission](#glow--emission)
5. [Water & Waves](#water--waves)
6. [Color Grading](#color-grading)
7. [Particle Shaders](#particle-shaders)

---

## Noise Functions

### Simplex 2D Noise (compact)
```glsl
vec3 permute(vec3 x) { return mod(((x*34.0)+1.0)*x, 289.0); }

float snoise(vec2 v) {
  const vec4 C = vec4(0.211324865405187, 0.366025403784439,
                      -0.577350269189626, 0.024390243902439);
  vec2 i  = floor(v + dot(v, C.yy));
  vec2 x0 = v - i + dot(i, C.xx);
  vec2 i1 = (x0.x > x0.y) ? vec2(1.0, 0.0) : vec2(0.0, 1.0);
  vec4 x12 = x0.xyxy + C.xxzz;
  x12.xy -= i1;
  i = mod(i, 289.0);
  vec3 p = permute(permute(i.y + vec3(0.0, i1.y, 1.0))
                          + i.x + vec3(0.0, i1.x, 1.0));
  vec3 m = max(0.5 - vec3(dot(x0,x0), dot(x12.xy,x12.xy),
                           dot(x12.zw,x12.zw)), 0.0);
  m = m*m; m = m*m;
  vec3 x = 2.0 * fract(p * C.www) - 1.0;
  vec3 h = abs(x) - 0.5;
  vec3 ox = floor(x + 0.5);
  vec3 a0 = x - ox;
  m *= 1.79284291400159 - 0.85373472095314 * (a0*a0 + h*h);
  vec3 g;
  g.x = a0.x * x0.x + h.x * x0.y;
  g.yz = a0.yz * x12.xz + h.yz * x12.yw;
  return 130.0 * dot(m, g);
}
```

### FBM (Fractal Brownian Motion)
```glsl
float fbm(vec2 p, int octaves) {
  float value = 0.0;
  float amplitude = 0.5;
  float frequency = 1.0;
  for (int i = 0; i < octaves; i++) {
    value += amplitude * snoise(p * frequency);
    frequency *= 2.0;
    amplitude *= 0.5;
  }
  return value;
}
```

---

## Vertex Displacement

### Terrain from noise
```glsl
// vertex shader
uniform float uTime;
uniform float uAmplitude;

void main() {
  vec3 pos = position;
  float elevation = fbm(pos.xz * 0.1 + uTime * 0.05, 6) * uAmplitude;
  pos.y += elevation;

  // Recalculate normal for lighting
  float eps = 0.01;
  float nx = fbm((pos.xz + vec2(eps, 0.0)) * 0.1, 6) * uAmplitude;
  float nz = fbm((pos.xz + vec2(0.0, eps)) * 0.1, 6) * uAmplitude;
  vec3 newNormal = normalize(vec3(elevation - nx, eps, elevation - nz));

  vNormal = normalMatrix * newNormal;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
}
```

### Sine wave displacement (water-like)
```glsl
uniform float uTime;
varying float vElevation;

void main() {
  vec3 pos = position;
  float wave1 = sin(pos.x * 2.0 + uTime) * 0.15;
  float wave2 = sin(pos.z * 3.0 + uTime * 0.8) * 0.1;
  float wave3 = sin((pos.x + pos.z) * 1.5 + uTime * 1.2) * 0.08;
  pos.y += wave1 + wave2 + wave3;
  vElevation = pos.y;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
}
```

---

## Fog & Atmosphere

### Exponential fog (fragment shader)
```glsl
uniform vec3 uFogColor;
uniform float uFogDensity;
uniform float uFogOffset;

vec3 applyFog(vec3 color, float dist) {
  float fogAmount = 1.0 - exp(-max(dist - uFogOffset, 0.0) * uFogDensity);
  return mix(color, uFogColor, fogAmount);
}
```

### Height-based fog
```glsl
vec3 applyHeightFog(vec3 color, vec3 worldPos, vec3 cameraPos) {
  float dist = length(worldPos - cameraPos);
  float heightFactor = smoothstep(0.0, 50.0, worldPos.y);
  float fogAmount = (1.0 - exp(-dist * 0.01)) * (1.0 - heightFactor);
  return mix(color, uFogColor, fogAmount);
}
```

---

## Glow & Emission

### Fresnel glow (edge glow effect)
```glsl
// fragment shader
varying vec3 vNormal;
varying vec3 vViewDir;

void main() {
  float fresnel = pow(1.0 - abs(dot(vNormal, vViewDir)), 3.0);
  vec3 baseColor = vec3(0.1, 0.1, 0.15);
  vec3 glowColor = vec3(0.2, 0.5, 1.0);
  vec3 color = mix(baseColor, glowColor, fresnel);
  gl_FragColor = vec4(color, 1.0);
}
```

### Pulsing emission
```glsl
uniform float uTime;
float pulse = 0.5 + 0.5 * sin(uTime * 2.0);
vec3 emission = uEmissionColor * pulse * uEmissionIntensity;
```

---

## Water & Waves

### Gerstner wave (more realistic than sine)
```glsl
vec3 gerstnerWave(vec2 pos, float steepness, float wavelength, vec2 direction, float time) {
  float k = 2.0 * 3.14159 / wavelength;
  float c = sqrt(9.8 / k);
  vec2 d = normalize(direction);
  float f = k * (dot(d, pos) - c * time);
  float a = steepness / k;

  return vec3(
    d.x * (a * cos(f)),
    a * sin(f),
    d.y * (a * cos(f))
  );
}
```

---

## Color Grading

### Film color grading (fragment post-process)
```glsl
vec3 filmGrade(vec3 color) {
  // Lift shadows, gamma midtones, gain highlights
  vec3 lift = vec3(0.02, 0.01, 0.05);   // slight blue in shadows
  vec3 gamma = vec3(1.0, 0.98, 0.95);    // warm midtones
  vec3 gain = vec3(1.05, 1.0, 0.95);     // warm highlights

  color = color * gain + lift;
  color = pow(color, 1.0 / gamma);
  return clamp(color, 0.0, 1.0);
}
```

### Vignette
```glsl
vec3 applyVignette(vec3 color, vec2 uv, float intensity) {
  float dist = length(uv - 0.5) * 1.414;
  float vignette = smoothstep(0.8, 0.2, dist * intensity);
  return color * vignette;
}
```

---

## Particle Shaders

### Point sprite with size attenuation
```glsl
// vertex
attribute float aSize;
attribute float aAlpha;
varying float vAlpha;

void main() {
  vAlpha = aAlpha;
  vec4 mvPosition = modelViewMatrix * vec4(position, 1.0);
  gl_PointSize = aSize * (300.0 / -mvPosition.z);
  gl_Position = projectionMatrix * mvPosition;
}

// fragment
varying float vAlpha;
uniform sampler2D uTexture;

void main() {
  vec4 texColor = texture2D(uTexture, gl_PointCoord);
  gl_FragColor = vec4(texColor.rgb, texColor.a * vAlpha);
  if (gl_FragColor.a < 0.01) discard;
}
```

### Soft circular particle (no texture needed)
```glsl
// fragment
varying float vAlpha;
uniform vec3 uColor;

void main() {
  float dist = length(gl_PointCoord - vec2(0.5));
  float alpha = smoothstep(0.5, 0.2, dist) * vAlpha;
  gl_FragColor = vec4(uColor, alpha);
}
```
