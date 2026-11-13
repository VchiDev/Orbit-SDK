# Orbit SDK 1.3.0

Orbit is a general-purpose programming language SDK with a tokenizer, parser, bytecode compiler, virtual machine, project packager, Lux library registry, and a Python-standard-library host runtime.

Orbit is designed for everyday applications and indie games. The SDK includes its existing compiler and VM, working Lux libraries, Flux Engine's playable 2D game loop, desktop app helpers, project packaging, and Orbit Nano for lightweight app distribution.

## Requirements

- Python 3.10 or newer.
- No third-party Python packages are required for the compiler, VM, standard libraries, software renderer, WAV writer, SQLite adapter, or office exporters.
- Tkinter is required only for native desktop windows and widgets. A GUI session is also required to display them.

## Install on Windows

1. Extract `Orbit-SDK-1.3.0` to a permanent location, for example `C:\Orbit`.
2. Open PowerShell in that folder.
3. Run:

```powershell
Set-ExecutionPolicy -Scope Process Bypass
.\install.ps1
```

4. Open a new PowerShell window and verify:

```powershell
orbit --version
orbitc --version
orbitvm --version
orbitpkg --version
orbitnano --version
lux list
```

The installer adds the SDK's `bin` folder to the current user's PATH. It does not install Python for you.

## First program

```powershell
orbitc .\examples\hello.orb
orbitvm .\examples\hello.beam
```

Or run source directly:

```powershell
orbit run .\examples\hello.orb
```

## Create a project

```powershell
orbit new MyConsoleApp --template console-app
cd MyConsoleApp
orbit run .\src\main.orb
orbitpkg build .
orbitpkg run .\MyConsoleApp.orbit
```

Game starter template:

```powershell
orbit new MyGame --template game-prototype
cd MyGame
orbit run .\src\main.orb
```

`flux.run(game, width, height, title, fps)` is the interactive game entry point. It opens a window, starts a frame loop, processes keyboard input, updates entities, handles collectible collisions and draws the live scene. In contrast, `flux.render(scene, canvas)` only draws one frame to an off-screen graphics canvas; pair that with `graphics.save_png` when you specifically want an image file.

The game template opens a playable desktop game window. Move with Arrow keys or WASD, collect all four crystals, and press Escape to exit.

## Command reference

| Command | Purpose |
|---|---|
| `orbit version` | Show SDK version |
| `orbit new NAME --template console-app` | Create a console project |
| `orbit new NAME --template game-prototype` | Create a Flux Engine starter |
| `orbit run file.orb` | Compile and execute Orbit source |
| `orbit compile file.orb` | Compile source to BEAM |
| `orbit vm file.beam` | Run BEAM bytecode |
| `orbit package build [project]` | Build an `.orbit` application archive |
| `orbit package run app.orbit` | Run a packaged app |
| `orbit package inspect app.orbit` | Inspect a package manifest and files |
| `orbitnano build app.orbit` | Build a compact player runtime with only the app's built-in libraries |
| `orbit fmt file.orb` | Trim trailing whitespace and normalize final newline |
| `orbit fmt file.orb --check` | Check formatting without modifying |
| `orbit test` | Run SDK tests |
| `orbit memory encode TEXT` | Encode text using Orbit Memory Code |
| `orbit memory decode CODE` | Decode Orbit Memory Code |
| `orbit memory address N` | Encode a 16-bit memory address |
| `lux list` | List Lux libraries |
| `lux inspect orbit.math` | Inspect a Lux manifest |
| `lux validate orbit.math` | Validate a Lux manifest |

The global launchers `orbit`, `orbitc`, `orbitvm`, `orbitpkg`, `orbitnano`, and `lux` resolve the SDK relative to their own location, so they work from any current directory after installation.

## Language preview

```orbit
fn square(value) {
    return value * value
}

var numbers = [1, 2, 3, 4]
var profile = {name: "Orbit", active: true}
var total = 0

for number in numbers {
    total += square(number)
}

if profile["active"] {
    signal("Total: " + str(total))
}
```

The language includes mutable and constant bindings, arithmetic, comparison and boolean operators, functions, parameters, returns, lists, object/dictionary literals, indexing, if/else, while, for/in, and namespaced library calls. See `docs/LANGUAGE.md`.

## Executable library capabilities

The `.lux` files are now versioned library metadata backed by Python host implementations, not empty API declarations. Examples:

- `graphics`: RGBA canvas, filled/outlined rectangles, circles, lines, polygons, bitmap text, PNG export.
- `graphics3d`: cube/custom mesh data, transforms, perspective projection, flat-shaded CPU triangle rendering and wireframes.
- `fluxengine`: interactive 2D game window, frame loop, keyboard movement, entity motion, collectible collisions, scoring and win states; plus headless scene rendering.
- `physics`: body integration, gravity/forces, circle/AABB overlap and bounce response.
- `audio`: WAV tone synthesis, WAV inspection and optional OS playback.
- `office`: document model/HTML export, spreadsheet cells and basic formulas, slide deck/HTML export, minimal one-page PDF text export.
- `database`: SQLite connections, parameterized SQL execution and row queries.
- `network`: HTTP/HTTPS GET, POST, HEAD and bounded downloads.
- `files`, `data`, `text`, `math`, `time`, `system`, `security`, `console`, `debug`, `animation` are implemented with documented subsets.
- `window`, `ui` and interactive `input` use Tkinter when available; they are optional and need a desktop session.

All APIs are discoverable in `docs/STANDARD_LIBRARIES.md`. `examples/` includes working samples.

## Important capability boundaries

Orbit uses its existing `.beam` bytecode and VM. Graphics uses a dependency-free CPU renderer for 2D drawing and simple low-poly 3D rendering. Flux Engine provides a windowed 2D game loop for indie projects. Physics includes practical 2D body and collision helpers. Audio can synthesize WAV clips and delegate playback to an available system player. Office tools provide document, spreadsheet, presentation and basic PDF workflows. Desktop windows and widgets use Tkinter, so those features require a working desktop session.

There is no static type checker, optimizer, garbage collector, native FFI, async scheduler, class/inheritance system, visual editor, package registry, cryptographic package signing, or production debugger/profiler yet. These are not advertised as present.

## Repository structure

See `docs/ARCHITECTURE.md` for the actual SDK layout, compiler/runtime flow, and implementation status by subsystem.

## Built-in Lux library cookbook

Every built-in library has a `.lux` manifest in `libraries/` and a runtime implementation. Add `lux.loadLib(...)` near the top of your source file to declare the dependency. Namespaced calls use the runtime namespace shown below.

### Console — `orbit.console` (`console`)

```orbit
lux.loadLib(orbit.console.lux)
console.write("Saving ")
console.log("complete")
var answer = console.color("Orbit is ready", "36")
signal(answer)
```

### Math — `orbit.math` (`math`)

```orbit
lux.loadLib(orbit.math.lux)
var direction = math.normalize(math.vec2(3, 4))
signal(math.sqrt(81))
signal(math.lerp(0, 100, 0.25))
signal(direction)
```

### Text — `orbit.text` (`text`)

```orbit
lux.loadLib(orbit.text.lux)
var title = text.trim("  orbit sdk  ")
signal(text.upper(title))
signal(text.replace("game-development", "-", " "))
signal(text.codepoints("Orbit"))
```

### Data — `orbit.data` (`data`)

```orbit
lux.loadLib(orbit.data.lux)
var profile = data.json_parse("{\"name\":\"Orbit\",\"level\":4}")
profile["level"] = profile["level"] + 1
signal(data.json_stringify(profile, true))
var unique_scores = data.unique([10, 10, 20, 30])
signal(unique_scores)
```

### Files — `orbit.files` (`files`)

```orbit
lux.loadLib(orbit.files.lux)
files.mkdir("save")
files.write_text("save/player.txt", "Explorer\nLevel 1")
signal(files.read_text("save/player.txt"))
signal(files.list_dir("save"))
```

### Time — `orbit.time` (`time`)

```orbit
lux.loadLib(orbit.time.lux)
var started = time.measure_start()
time.sleep(0.05)
signal(time.elapsed(started))
signal(time.date_string())
```

### System — `orbit.system` (`system`)

```orbit
lux.loadLib(orbit.system.lux)
signal(system.os_name())
signal(system.architecture())
signal(system.cwd())
var app_mode = system.env("ORBIT_MODE", "normal")
signal(app_mode)
```

### Security — `orbit.security` (`security`)

```orbit
lux.loadLib(orbit.security.lux)
signal(security.sha256("Orbit save data"))
var session_token = security.secure_token(24)
signal(session_token)
signal(security.constant_time_equal("same", "same"))
```

### Networking — `orbit.network` (`network`)

```orbit
lux.loadLib(orbit.network.lux)
// Requires an internet connection. Responses are bounded by the runtime.
var response = network.get("https://example.com", 10)
signal(response["status"])
signal(response["body"])
```

### Graphics 2D — `orbit.graphics` (`graphics`)

```orbit
lux.loadLib(orbit.graphics.lux)
var frame = graphics.canvas(640, 360, "#17243b")
graphics.rect(frame, 40, 50, 180, 90, "#ffcc66")
graphics.circle(frame, 320, 150, 48, "#65e8ff")
graphics.text(frame, 24, 20, "Orbit Canvas", "#ffffff", 2)
graphics.save_png(frame, "build/canvas.png")
```

### Graphics 3D — `orbit.graphics3d` (`graphics3d`)

```orbit
lux.loadLib(orbit.graphics.lux)
lux.loadLib(orbit.graphics3d.lux)
var frame = graphics.canvas(480, 320, "#101827")
var cube = graphics3d.cube(1.5)
cube = graphics3d.transform(cube, [0.4, 0.7, 0.2], [0, 0, 4])
graphics3d.render_wireframe(frame, cube, "#8bd5ff", 5, 90)
graphics.save_png(frame, "build/cube.png")
```

### Flux Engine — `orbit.fluxengine` (`flux`)

```orbit
lux.loadLib(orbit.fluxengine.lux)
var game = flux.scene("My Indie Game", "#17243b")
var player = flux.entity(game, "Player", 40, 80, 32, 48, "#ffcc66", "rect")
flux.control(game, player, 220, "arrows_wasd")
flux.collectible(game, "Gem", 220, 120, 20, "#65e8ff")
flux.goal(game, 1, "You win!")
flux.run(game, 640, 360, "My Indie Game", 60)
```

### Physics — `orbit.physics` (`physics`)

```orbit
lux.loadLib(orbit.physics.lux)
var ball = physics.body(10, 0, 0, 0, 1, 8)
physics.apply_force(ball, 0, 9.8)
physics.integrate(ball, 0.016, 0, 9.8)
signal(ball["y"])
```

### Animation — `orbit.animation` (`animation`)

```orbit
lux.loadLib(orbit.animation.lux)
var keys = [animation.keyframe(0, 0), animation.keyframe(1, 100)]
signal(animation.timeline(keys, 0.5, "ease_in_out"))
signal(animation.ping_pong(2.4, 1.0))
```

### Audio — `orbit.audio` (`audio`)

```orbit
lux.loadLib(orbit.audio.lux)
var clip = audio.tone("build/coin.wav", 880, 0.25, 0.2, 44100, "sine")
signal(clip["duration"])
signal(audio.info("build/coin.wav"))
// audio.play("build/coin.wav") uses the operating system's available player.
```

### Window — `orbit.window` (`window`)

```orbit
lux.loadLib(orbit.window.lux)
var app = window.create("Orbit Desktop App", 520, 340)
window.label(app, "Welcome to Orbit", 24, 24)
window.run(app)
```

### UI — `orbit.ui` (`ui`)

```orbit
lux.loadLib(orbit.window.lux)
lux.loadLib(orbit.ui.lux)
var app = window.create("Settings", 420, 260)
var enabled = ui.checkbox(app, "Enable sound", true, 20, 20)
var name = ui.entry(app, 20, 70, 220)
ui.text_set(name, "Player One")
window.run(app)
signal(ui.checked(enabled))
```

### Input — `orbit.input` (`input`)

```orbit
lux.loadLib(orbit.window.lux)
lux.loadLib(orbit.input.lux)
var app = window.create("Input Test", 360, 220)
input.bind_tk(app)
window.label(app, "Click this window, then press a key. Close to finish.", 12, 12)
window.run(app)
signal(input.mouse_position())
```

### Database — `orbit.database` (`database`)

```orbit
lux.loadLib(orbit.database.lux)
var db = database.open_database("save/profile.db")
database.execute(db, "CREATE TABLE IF NOT EXISTS scores (name TEXT, points INTEGER)")
database.execute(db, "INSERT INTO scores VALUES (?, ?)", ["Explorer", 120])
signal(database.query(db, "SELECT name, points FROM scores"))
database.close(db)
```

### Debug — `orbit.debug` (`debug`)

```orbit
lux.loadLib(orbit.debug.lux)
var score = 125
debug.assert_true(score >= 0, "Score cannot be negative")
signal(debug.inspect({score: score, type: debug.type_of(score)}))
```

### Desktop — `orbit.desktop` (`desktop`)

```orbit
lux.loadLib(orbit.desktop.lux)
var app = desktop.create("Quick Note", 500, 320)
desktop.label(app, "A tiny desktop application", 20, 20)
desktop.run(app)
```

### Office — `orbit.office` (`office`)

```orbit
lux.loadLib(orbit.office.lux)
var doc = office.document("Project Notes", "Orbit User")
office.heading(doc, "Sprint Summary", 1)
office.paragraph(doc, "The first playable prototype is ready.")
office.export_html(doc, "build/notes.html")
var sheet = office.sheet(10, 5)
office.cell_set(sheet, "A1", 12)
office.cell_set(sheet, "A2", 30)
office.cell_set(sheet, "A3", "=SUM(A1:A2)")
office.export_csv(sheet, "build/scores.csv")
```

## Building a custom Lux library

Custom libraries are application-owned: keep their `.lux` manifest and `.orb` source in the project `lib/` folder. The `.orbit` builder copies that folder into the application package and generates an adjacent `.beam` module for Nano's VM. Orbit Nano does **not** copy custom code into its shared built-in library system; the custom library stays with the `.orbit` app, where the app can load it.

Project layout:

```text
MyApp/
  orbit.json
  src/main.orb
  lib/geometry.lux
  lib/geometry.orb
```

`lib/geometry.lux`:

```text
LUX-LIBRARY 01
NAME myapp.geometry
VERSION 1.0.0
STATUS IMPLEMENTED
DESCRIPTION Small geometry helpers used by MyApp
NAMESPACE geometry
SOURCE geometry.orb
MODULE geometry_functions
EXPORT square
EXPORT distance_2d
```

`lib/geometry.orb`:

```orbit
fn square(value) {
    return value * value
}

fn distance_2d(x1, y1, x2, y2) {
    var dx = x2 - x1
    var dy = y2 - y1
    return math.sqrt(square(dx) + square(dy))
}
```

`src/main.orb`:

```orbit
lux.loadLib(orbit.math.lux)
lux.loadLib("lib/geometry.lux")
signal(geometry.square(9))
signal(geometry.distance_2d(0, 0, 3, 4))
```

Build and run:

```powershell
orbitpkg build .
orbitpkg inspect .\MyApp.orbit
orbitpkg run .\MyApp.orbit
orbitnano build .\MyApp.orbit -o .\MyApp-Nano.zip
```

When `orbitnano build` runs, the Nano bundle receives only the built-in library runtime modules declared by the app. Custom `.lux` manifests, `.orb` sources and `.beam` modules remain inside `MyApp.orbit` in `lib/`. Nano contains the compact VM runtime and does not include the Orbit compiler.


## Embedding Orbit Nano directly into an application

Add `orbit.LoadNano()` to the main Orbit source file to request a self-contained Nano runtime inside the `.orbit` package:

```orbit
orbit.LoadNano() // build-time directive; no runtime download or SDK install
lux.loadLib(orbit.fluxengine.lux)
lux.loadLib(orbit.graphics.lux)
signal("Hello from my embedded Orbit app")
```

Then build and run:

```powershell
orbitpkg build . -o .\build\MyApp.orbit
orbitpkg inspect .\build\MyApp.orbit
orbitpkg run .\build\MyApp.orbit
```

When `orbit.LoadNano()` is present, `orbitpkg build` embeds the compact VM, only the runtime modules needed by referenced built-in Lux libraries, their `.lux` manifests, and metadata under `nano/`. The application package also preserves the complete project `assets/` directory, including nested folders and files. Custom Lux libraries, their sources and compiled `.beam` modules remain in the app-local `lib/` folder; they are not copied into Nano's shared built-in library area.

A typical embedded application archive contains:

```text
MyApp.orbit
├── orbit.json
├── main.beam
├── assets/                 # all app assets, including subfolders
├── lib/                    # custom Lux libraries and compiled modules
└── nano/
    ├── runtime/            # compact VM and selected host modules
    ├── libraries/          # only built-in Lux manifests used by the app
    ├── metadata/
    │   ├── nano.json
    │   ├── dependencies.json
    │   ├── compatibility.json
    │   └── build.json
    ├── orbit_stdlib.py
    └── run_embedded.py
```

`orbit.LoadNano()` is a packaging directive. It compiles to a harmless bytecode marker; it does not copy files while the game is running. Apps without the directive remain ordinary `.orbit` packages and do not include Nano. `orbit run MyApp.orbit` or `orbitpkg run MyApp.orbit` detects embedded Nano and starts the package through the embedded compact VM. To run without installing the full SDK, extract the ZIP-format `.orbit` file, then run `nano/run.ps1` on Windows or `nano/run.sh` on Linux/macOS. This source-runtime release still requires Python 3.10+ on the player's system; it does not yet produce a native standalone executable.

The existing `orbitnano build` command remains available for creating a separate Nano player bundle when developers explicitly want a separate runtime distribution.

## Orbit Nano distribution model

Build a normal `.orbit` app with the full SDK, then create a player runtime:

```powershell
orbitpkg build .
orbitnano build .\MyApp.orbit -o .\MyApp-Nano.zip
```

Orbit Nano is a VM-only launcher bundle. Built-in libraries used by the app are embedded into Nano's `runtime/stdlib/`, `engine/`, or `office/` files and their matching built-in `.lux` manifests are included in Nano's `libraries/` directory. A custom library is never moved into the Nano shared runtime: its `.lux` manifest, `.orb` source and generated `.beam` module stay under `lib/` inside the `.orbit` app. Nano extracts the app to a temporary app directory when it runs, then loads custom modules from that app's own `lib/` folder. Players do not need the full Orbit SDK or compiler. They do need Python 3.10+ for this source-runtime build.

## Building and running a real Flux game

The game template is a playable game project, not a screenshot generator. Its `src/main.orb` calls `flux.run(...)`, which opens the native game window and keeps the game running until the player exits. Keyboard controls, movement, collision collection, score display and a win message are provided by the Flux game loop. The earlier canvas/PNG pattern is still supported, but it is an off-screen one-frame render rather than interactive gameplay.

## Orbit syntax reference

Orbit source files use UTF-8 and `.orb`. Newlines or semicolons separate statements. Comments start with `//` or `#`.

```orbit
// Values and bindings
const APP_NAME = "Orbit Notes"
let is_ready = true
var count = 0
var values = [1, 2, 3]
var profile = {name: "Ari", active: true}

// Assignment, including indexed and object-member updates
count += 1
values[0] = 10
profile.name = "Player"

// Operators: + - * / %, == != < <= > >=, in, and/or, &&/||, !/not
if is_ready and count in [0, 1, 2] {
    signal(profile.name)
} else {
    signal("Not ready")
}

// Functions, returns, loops, break and continue
fn sum(items) {
    var total = 0
    for item in items {
        if item < 0 { continue }
        total += item
    }
    return total
}

var index = 0
while index < 10 {
    index += 1
    if index == 5 { break }
}
signal(sum(values))
```

The language core supports mutable/constant bindings, null/boolean/integer/float/string values, list and object literals, indexing, member access, assignment, function definitions and calls, recursion, returns, if/else, while, for/in, break/continue, arithmetic/comparison/boolean/membership operators, and namespaced library calls. Function-local variables can read globals; closures and nested lexical captures are not part of the current language. There are no classes, exceptions, async/await, lambdas, or user-defined type declarations in this release. Boolean `and`/`or` use short-circuit evaluation.
