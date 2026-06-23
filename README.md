# 🏳️ Flag Studio — 3D Waving Flags

A single-file, in-browser 3D studio that renders **cloth-simulated waving flags** for all 48 FIFA World Cup 2026 nations — with live wind controls, PBR fabric, and one-click PNG / ZIP export.

**Live demo:** https://hervestudio.github.io/flag-studio/

Forked from the Soccer Ball — 3D Texture Studio, keeping its team picker, lighting rig, and export plumbing — but the soccer ball is replaced by a flag on a pole.

## What it does

- **Real cloth motion.** A finely-tessellated plane is displaced in a vertex shader by a sum of travelling sine waves, with the amplitude growing from the pole to the free edge. **Normals are recomputed analytically in the shader**, so the fabric catches light correctly — full `MeshStandardMaterial` PBR with environment reflections.
- **Woven fabric.** A procedural, tileable plain-weave **normal map** (over/under warp + weft threads) is applied to the cloth so it reads as real woven fabric, not plastic. Strength is adjustable ("Weave normal").
- **Real flags.** Each nation's vector flag (the [flag-icons](https://github.com/lipis/flag-icons) 4×3 set) is rendered to a high-res canvas and mapped onto the cloth. A vendored `_flags/<code>.svg` is used when present; otherwise it's fetched from the CDN.
- **48 nations.** A floating picker (confederation tabs + search) swaps flags with a cross-fade and a brief wind gust.
- **Switchable, rotatable environment.** A procedural studio or real [Poly Haven](https://polyhaven.com) HDRIs (PMREM-prefiltered) light the scene; rotate the lighting, and optionally show the HDRI as a backdrop.
- **Camera rig.** Adjustable focal length (18–200mm) and an optional depth-of-field pass (BokehPass, with an aperture control and a focus point you set — Auto on the camera's aim point, a manual distance slider, or **double-click the flag** to focus there). The DOF depth pass runs the same cloth wave as the render, so the blur tracks the real surface instead of a flat plane. Live X/Y/Z position + target fields you can edit directly, plus a "Copy camera" button that yields ready-to-paste `camera.position.set(...) / controls.target.set(...)` code. Press **C** to detach into a free-orbit *editor* view: the shot camera becomes a draggable body + frustum with a transform gizmo (**G** to move, **R** to rotate) and a handle to re-aim it — and the **flag itself** gets a gizmo too, so you can move/rotate it in the same mode. A live **picture-in-picture** shows exactly what the shot camera sees while you work.
- **Smooth flag swaps.** Changing nation sweeps the incoming flag's texture across the cloth (a single-mesh shader wipe with a wind gust) — no overlap, no fade artefacts.
- **Extend to fill.** An *Extend* control grows the cloth past its edges, with the flag's outer bands continuing outward (clamp-to-edge) and the wave going free of the pole — so close/cinematic shots fill the frame with flag instead of black background.
- **Live controls.** Wind strength / speed / ripple detail / frequency / curl, flag proportion (3:2, 5:3, 2:1, 4:3, vertical 2:3), flagpole on/off, extend-to-fill, fabric roughness / sheen / weave strength / weave scale, full lighting rig, and a background colour.
- **Export.** Download the current flag image, a PNG of exactly what you see (the render), or **every nation's flag as a single ZIP**.

## Run locally

One file, no build step. Serve it (ES modules need http, not `file://`):

```bash
python3 -m http.server 8754
# open http://localhost:8754
```

Three.js and the flag set load from CDNs via an import map / fetch, so an internet connection is needed (the three vendored flags in `_flags/` work offline).

## Tech

Vanilla JS + [Three.js](https://threejs.org/) (CDN). The wave + normals are injected into the standard material via `onBeforeCompile`; flags are drawn on `<canvas>` and applied as a `CanvasTexture`. The "all flags" ZIP uses a tiny built-in store-only ZIP writer.

🤖 Built with [Claude Code](https://claude.com/claude-code)
