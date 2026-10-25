# Retro Textures

A browser tool for making PS1 / N64-style tileable textures: low-res, palette-limited, seamless, and animated in exact loops. It's the testbed for a future FFGL plugin for Resolume.

Open `index.html` in a browser with WebGL2 (or serve the folder, e.g. `npx serve .`). It's a single file with no dependencies.

## What it does

- **35 material presets** (stone, brick, tiles, dirt, grass, water, lava, fire, wood, rock faces, clouds, sky backdrops and more), built from 16 shader materials driven by periodic noise.
- **Game-era look:** height-map lighting, blended or indexed palettes, tint patches, grain, ordered dither, 15-bit color, tile sizes from 16 to 128.
- **Overlay layer:** a second material grown over the base in patches, on high areas or in cracks.
- **Animation that loops exactly:** Evolve (noise development), Flow, Palette cycle and Scroll, with loop length set from BPM and beats.
- **Display views:** flat wall, perspective floor and PS1 affine floor, with nearest / N64 3-point / bilinear filtering, vertex (Gouraud) shading, fog and a sky above the horizon.
- **Randomize** (button or `R`), with Undo and Keep material.
- **Export:** tile PNG, sprite sheet, one-loop video recording and preset JSON.

## How it's built

All in `index.html`:

1. **GEN** builds one N×N tile. Each material returns height, tone and tint; a shared stage lights the height map and maps tone onto the palette.
2. **COMP** lays the optional overlay over the base through a periodic mask.
3. **DISP** tiles the result across a low-res frame (240p by default) with console-style sampling.
4. **BLIT** upscales it with nearest-neighbor.

Noise is periodic in x, y and time, and every animation amount is a whole number per loop, so the tile wraps seamlessly and the last frame of a loop matches the first pixel for pixel. The shaders take separate motion and evolve phases, ready for the FFGL plugin's phase parameters.
