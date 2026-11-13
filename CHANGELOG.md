# Changelog

## 1.3.0 — 2026-11-12

- Added `orbit.LoadNano()` as a compiler-recognized packaging directive.
- `orbitpkg build` now embeds a dependency-selected Nano runtime into `.orbit` applications when requested.
- Added Nano runtime, dependency, build and bytecode-compatibility metadata.
- Preserved the project `assets/` tree (including nested folders) in `.orbit` packages.
- `orbitpkg run` now detects and executes an embedded Nano runtime.
- Built-in Lux manifests are embedded selectively; custom libraries remain under the app-local `lib/` directory.

## 1.2.0 — 2026-11-12
- Added Flux Engine's interactive Tk game window and frame loop with arrow/WASD controls, entity motion, collectible collision, score and win feedback.
- Connected Flux keyboard and mouse events to the shared Orbit input library.
- Expanded the graphics3d library with flat-shaded CPU polygon rendering and mesh face support.
- Expanded the 2D physics helpers with AABB/circle collision resolution.
- Added a dedicated BEAM-only `runtime/compact_vm.py` for Orbit Nano; Nano no longer includes `orbit_tool.py` or the compiler.
- Orbit packages now contain precompiled `.beam` modules for custom Lux source while keeping custom `.lux`, `.orb` and `.beam` files under the app's `lib/` directory.
- Orbit Nano embeds only the built-in runtime modules and matching Lux manifests required by the app; custom libraries remain inside the `.orbit` app.
- Added the `orbitnano` launcher and expanded examples, syntax documentation, library cookbook and Flux game instructions.
- Corrected release metadata and API documentation to match the implemented behavior.

## 1.1.0 — 2026-11-10
- Added user-defined functions, parameters, return values, global reads from functions, list and object literals, indexing, and `for/in` loops.
- Expanded bytecode format to `ORBIT-BEAM-2` with function code, collection operations and runtime calls.
- Added executable host implementations for the standard library namespaces.
- Added dependency-free RGBA software drawing and PNG export, plus CPU 3D wireframe transforms/projection.
- Added Flux scene/entity updates and rendering, basic physics, WAV tone generation and optional playback.
- Added file, text, data, time, system, security, HTTP, SQLite, desktop Tk, and office document/sheet/presentation/PDF APIs.
- Added Orbit Memory Code encode/decode/address functions and CLI commands.
- Improved `.orbit` packaging, package extraction validation, working-directory handling and project templates.
- Added modular public entry points and expanded integration tests.

## 1.0.0 — 2026-11-09
- Established the tokenizer, expression parser, bytecode compiler, VM and package toolchain.
