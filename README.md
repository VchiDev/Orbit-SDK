# Orbit SDK 1.3.0
## The complete beginner's guide to installing, learning, building, and distributing with Orbit

Welcome to **Orbit**! This guide is for developers who are completely new to the Orbit programming language and SDK. It walks through installation, your first program, the compiler and virtual machine, the language, Lux libraries, desktop applications, 2D and simple 3D graphics, Flux Engine games, project packaging, Orbit Nano, troubleshooting, and the command-line tools.

Orbit is designed for **everyday applications, utilities, creative software, and indie games**. It is not intended to be an AAA game engine. Orbit uses its existing compiler and virtual machine and compiles source code into Orbit's `.beam` bytecode format.

> **Version note:** This guide is written for the Orbit SDK **1.3.0** package. Commands and APIs may differ in other versions. When in doubt, check the documentation shipped in your installed SDK's `docs/` folder.

---

# Table of contents

1. [What is Orbit?](#1-what-is-orbit)
2. [What is included in the SDK?](#2-what-is-included-in-the-sdk)
3. [Requirements](#3-requirements)
4. [Install Orbit on Windows](#4-install-orbit-on-windows)
5. [Install Orbit on Linux or macOS](#5-install-orbit-on-linux-or-macos)
6. [Verify your installation](#6-verify-your-installation)
7. [Your first Orbit program](#7-your-first-orbit-program)
8. [How Orbit runs a program](#8-how-orbit-runs-a-program)
9. [Orbit language tutorial](#9-orbit-language-tutorial)
10. [Functions and reusable code](#10-functions-and-reusable-code)
11. [Lists and objects](#11-lists-and-objects)
12. [Conditions and loops](#12-conditions-and-loops)
13. [Lux libraries](#13-lux-libraries)
14. [Create a project](#14-create-a-project)
15. [Build and inspect an `.orbit` package](#15-build-and-inspect-an-orbit-package)
16. [Orbit Nano: distribute apps without the full SDK](#16-orbit-nano-distribute-apps-without-the-full-sdk)
17. [Build a desktop application](#17-build-a-desktop-application)
18. [Build a 2D game with Flux Engine](#18-build-a-2d-game-with-flux-engine)
19. [Graphics and simple 3D](#19-graphics-and-simple-3d)
20. [Audio, data, files, networking, and office tools](#20-audio-data-files-networking-and-office-tools)
21. [Orbit Memory Code](#21-orbit-memory-code)
22. [Format, test, and debug your code](#22-format-test-and-debug-your-code)
23. [Command-line reference](#23-command-line-reference)
24. [Project folder reference](#24-project-folder-reference)
25. [PowerShell and shell scripts](#25-powershell-and-shell-scripts)
26. [Troubleshooting](#26-troubleshooting)
27. [Known limitations](#27-known-limitations)
28. [Where to go next](#28-where-to-go-next)

---

# 1. What is Orbit?

Orbit is a general-purpose programming language and software development kit. It includes:

- An Orbit source lexer and parser.
- A compiler that converts `.orb` source files into `.beam` bytecode.
- A virtual machine that executes BEAM programs.
- A project manager and `.orbit` application packager.
- Lux library manifests and a registry of built-in libraries.
- A Python-based host runtime for library functionality.
- Flux Engine, Orbit's practical 2D indie-game engine.
- Desktop window and widget helpers.
- Orbit Nano, a compact runtime bundle for distributing packaged applications.
- Example programs, templates, tests, and technical documentation.

Orbit is suitable for learning programming, creating command-line tools, building small desktop utilities, making creative applications, and developing indie games.

## Orbit file formats

| Extension | Purpose |
|---|---|
| `.orb` | Human-readable Orbit source code |
| `.beam` | Orbit bytecode / intermediate instruction file |
| `.lux` | Lux library manifest |
| `.orbit` | Packaged Orbit application archive |

An `.orbit` file is a ZIP-based application package. It is not the same thing as an `.orb` source file.

---

# 2. What is included in the SDK?

The SDK is organized into several main parts:

| Folder | What it contains |
|---|---|
| `bin/` | Command launchers such as `orbit`, `orbitc`, `orbitvm`, `orbitpkg`, `orbitnano`, and `lux` |
| `compiler/` | Lexer, parser, bytecode generation, and compiler diagnostics |
| `runtime/` | Orbit VM, values, function support, and runtime libraries |
| `libraries/` | Built-in `.lux` library manifests |
| `engine/flux/` | Flux Engine scene, game-loop, and window support |
| `desktop/` | Desktop window, widget, and input helpers |
| `office/` | Document, spreadsheet, presentation, and PDF helpers |
| `package_manager/` | Project manifests, dependency resolution, package building, and inspection |
| `examples/` | Ready-to-read and runnable Orbit programs |
| `templates/` | Starter templates for new projects |
| `docs/` | Detailed language, compiler, runtime, package, engine, and library documentation |
| `tests/` | SDK test suite |
| `tools/` | Formatter, debugger, and profiler utilities |

You normally do not need to edit the SDK's internal Python files to write Orbit programs. Start by creating `.orb` source files in your own projects.

---

# 3. Requirements

## Required

- **Python 3.10 or newer**
- Windows, Linux, or macOS
- A terminal:
  - PowerShell on Windows
  - A shell such as Bash or Zsh on Linux/macOS

The compiler, VM, core standard-library host, software graphics renderer, WAV writer, SQLite adapter, and office exporters are designed to work without third-party Python packages.

## Optional requirements

Some features need extra operating-system support:

| Feature | Additional requirement |
|---|---|
| Native desktop windows and widgets | Python Tkinter and a graphical desktop session |
| Flux Engine interactive game window | Tkinter and a graphical desktop session |
| Audio playback | `winsound` on Windows, `afplay` on macOS, or `aplay` on Linux |
| Network examples | Internet connection |
| Headless graphics output | No GUI required; use the software renderer and PNG export |

On some Linux distributions, Tkinter is packaged separately (often as `python3-tk`). Installing Python does not always install Tkinter.

Check Python:

```powershell
python --version
```

On Linux/macOS, use:

```sh
python3 --version
```

---

# 4. Install Orbit on Windows

These steps are for Windows PowerShell.

## Step 1 — Extract the SDK

1. Download `Orbit-SDK-1.3.0.zip`.
2. Right-click the ZIP file in File Explorer.
3. Choose **Extract All...**.
4. Extract it to a permanent folder.

For example:

```text
C:\Orbit\Orbit-SDK-1.3.0
```

Avoid leaving the SDK in a temporary folder that you may later delete.

## Step 2 — Open PowerShell in the SDK folder

Open the extracted `Orbit-SDK-1.3.0` folder in File Explorer.

Click the address bar, type:

```text
powershell
```

Press Enter. PowerShell should open in the SDK folder.

Check the current directory:

```powershell
Get-Location
```

You should see the folder where `install.ps1` is located.

## Step 3 — Run the installer

PowerShell may block local scripts by default. The following command temporarily permits scripts in the current PowerShell process only:

```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
```

Now run the installer:

```powershell
.\install.ps1
```

The installer:

1. Finds the SDK folder.
2. Checks that Python is installed.
3. Checks that Python is version 3.10 or newer.
4. Adds the SDK's `bin` directory to your **user PATH** if it is not already there.

The installer does **not** install Python for you.

## Step 4 — Open a new PowerShell window

Close the current PowerShell window and open a new one. This allows Windows to load the updated PATH.

Continue to [Verify your installation](#6-verify-your-installation).

## Uninstall Orbit's PATH entry

From the SDK folder, run:

```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
.\uninstall.ps1
```

This removes the SDK's `bin` directory from the user PATH. It does **not** delete the SDK folder or your projects. You can delete the SDK folder yourself after uninstalling if you no longer need it.

---

# 5. Install Orbit on Linux or macOS

Open a terminal in the extracted SDK folder.

Make the installer executable and run it:

```sh
chmod +x ./install.sh
./install.sh
```

The installer adds the SDK's `bin` directory to `~/.profile` if it is not already present.

Open a new terminal, or reload the profile:

```sh
source ~/.profile
```

Check that Python is available:

```sh
python3 --version
```

If your system uses `python` for Python 3, that command may also work:

```sh
python --version
```

The installer does not install Python. You must install Python 3.10 or newer using your operating system's normal package manager or installer.

Interactive desktop and game windows need Tkinter and a graphical session. On Linux, you may need to install your distribution's Tkinter package separately.

---

# 6. Verify your installation

Open a **new** terminal after installation.

On Windows PowerShell:

```powershell
orbit --version
orbitc --version
orbitvm --version
orbitpkg --version
orbitnano --version
lux list
```

On Linux/macOS, the same commands should work after the PATH has been updated:

```sh
orbit --version
orbitc --version
orbitvm --version
orbitpkg --version
orbitnano --version
lux list
```

The version commands should identify the installed Orbit tools. `lux list` should print the built-in Lux libraries.

If a command is not found, see [Troubleshooting](#26-troubleshooting).

## Why are there several commands?

Orbit has separate command-line entry points for common development tasks:

- `orbit` — convenient unified command.
- `orbitc` — compiler.
- `orbitvm` — bytecode virtual machine.
- `orbitpkg` — application package manager.
- `orbitnano` — compact runtime builder.
- `lux` — Lux library registry and manifest tools.

The launchers locate the SDK relative to their own installation location. This is why they are designed to work even when your terminal is currently in a different directory.

---

# 7. Your first Orbit program

Let's create a tiny program that prints a message.

## Step 1 — Create a source file

Make a folder for your practice programs:

```powershell
mkdir OrbitPractice
cd OrbitPractice
```

On Linux/macOS:

```sh
mkdir OrbitPractice
cd OrbitPractice
```

Create a file named:

```text
hello.orb
```

Open it in a text editor and write:

```orbit
signal("Hello, Orbit!")
```

Save the file.

## Step 2 — Run it directly

From the folder containing `hello.orb`, run:

```powershell
orbit run .\hello.orb
```

On Linux/macOS:

```sh
orbit run ./hello.orb
```

Expected output:

```text
Hello, Orbit!
```

The unified `orbit run` command compiles the source and executes it.

## Step 3 — Compile it manually

You can also run the compiler separately:

```powershell
orbitc .\hello.orb
```

This creates:

```text
hello.beam
```

The `.beam` file is Orbit bytecode.

## Step 4 — Run the bytecode

```powershell
orbitvm .\hello.beam
```

This asks the Orbit virtual machine to execute the compiled program.

The full workflow is:

```text
hello.orb  --orbitc-->  hello.beam  --orbitvm-->  program output
```

You can repeat this workflow whenever you want to separate compilation from execution.

---

# 8. How Orbit runs a program

Orbit uses a source-to-bytecode pipeline:

1. **Source:** You write code in a `.orb` file.
2. **Lexer:** The lexer converts source text into tokens.
3. **Parser:** The parser checks the program's structure and creates its internal representation.
4. **Compiler:** The compiler emits Orbit BEAM instructions.
5. **Virtual machine:** The VM executes those instructions.
6. **Host libraries:** When your program calls supported built-in functions or Lux libraries, the runtime provides the corresponding functionality.

Orbit's `.beam` format is an intermediate bytecode format for the Orbit VM. It is not native CPU machine code.

For deeper technical details, see:

- `docs/ARCHITECTURE.md`
- `docs/COMPILER.md`
- `docs/RUNTIME.md`

---

# 9. Orbit language tutorial

This section introduces the language using small examples. Save each example as a `.orb` file and run it with `orbit run`.

## Comments

Use `//` or `#` for a single-line comment:

```orbit
// This is a comment.
# This is also a comment.

signal("Comments are ignored by the program.")
```

## Output

Use `signal(...)` to display a value:

```orbit
signal("Hello!")
signal(123)
signal(true)
```

## Variables

Use `var` for a mutable variable:

```orbit
var playerName = "Explorer"
var score = 0

score = score + 10
signal(playerName)
signal(score)
```

You can also use `let` as a mutable binding.

Use `const` for a constant binding that cannot be reassigned:

```orbit
const gameTitle = "Crystal Hunt"
signal(gameTitle)
```

## Basic value types

Orbit supports values such as:

```orbit
var playerName = "Alex"   // string
var score = 25            // integer
var speed = 4.5           // floating-point number
var isPlaying = true      // boolean
var nothing = null        // null value
```

## Arithmetic

```orbit
var a = 10
var b = 3

signal(a + b)
signal(a - b)
signal(a * b)
signal(a / b)
signal(a % b)
```

## Comparisons and boolean logic

```orbit
var score = 80

signal(score > 50)
signal(score == 80)
signal(score != 0)

var hasKey = true
var doorOpen = false

if hasKey and not doorOpen {
    signal("The key can open the door.")
}
```

Supported operators include:

| Category | Operators |
|---|---|
| Arithmetic | `+`, `-`, `*`, `/`, `%` |
| Comparison | `==`, `!=`, `<`, `<=`, `>`, `>=` |
| Boolean | `and`, `or`, `not`, `&&`, `||`, `!` |
| Membership | `in` |
| Assignment | `=`, `+=`, `-=`, `*=`, `/=` |

Parentheses can be used to make expression order clear:

```orbit
var result = (10 + 5) * 2
signal(result)
```

---

# 10. Functions and reusable code

Functions let you write a block of code once and call it multiple times.

```orbit
fn greet(name) {
    signal("Hello, " + name + "!")
}

greet("Orbit")
greet("Developer")
```

A function can return a value:

```orbit
fn square(number) {
    return number * number
}

var result = square(8)
signal(result)
```

Functions can call other functions:

```orbit
fn sum_to(count) {
    var total = 0

    for number in range(1, count + 1) {
        total += number
    }

    return total
}

signal(sum_to(5))
```

The example uses `range(start, end)` to produce a sequence of numbers. The ending value is not included.

---

# 11. Lists and objects

## Lists

Lists store multiple values:

```orbit
var inventory = ["Notebook", "Pencil", "Key"]

signal(inventory[0])
signal(inventory[1])

inventory[1] = "Blue Pencil"
signal(inventory)
```

List indexes start at zero.

## Objects / dictionaries

Objects store named values:

```orbit
var player = {
    name: "Explorer",
    score: 100,
    hasKey: true
}

signal(player["name"])
signal(player["score"])

player["score"] = player["score"] + 25
signal(player)
```

You can use string keys or identifier-style keys in object literals.

## Looping through a list

```orbit
var items = ["Map", "Notebook", "Coin"]

for item in items {
    signal(item)
}
```

---

# 12. Conditions and loops

## If / else

```orbit
var score = 75

if score >= 100 {
    signal("Gold rank")
} else if score >= 50 {
    signal("Silver rank")
} else {
    signal("Keep practicing")
}
```

## While loop

```orbit
var count = 0

while count < 3 {
    signal(count)
    count += 1
}
```

## For loop

```orbit
for number in range(1, 6) {
    signal(number)
}
```

## Break and continue

`break` exits the current loop. `continue` skips to the next iteration.

```orbit
for number in range(1, 10) {
    if number == 3 {
        continue
    }

    if number == 7 {
        break
    }

    signal(number)
}
```

## Complete language example

```orbit
fn square(value) {
    return value * value
}

var numbers = [1, 2, 3, 4]
var profile = {
    name: "Orbit",
    active: true
}
var total = 0

for number in numbers {
    total += square(number)
}

if profile["active"] {
    signal("Total: " + str(total))
}
```

For exact grammar details and current language boundaries, read `docs/LANGUAGE.md`.

---

# 13. Lux libraries

Lux is Orbit's library system. Built-in libraries have `.lux` manifests inside the SDK's `libraries/` directory and runtime implementations in the SDK.

Load a library near the top of your Orbit source:

```orbit
lux.loadLib(orbit.math.lux)
```

You can also use the alias:

```orbit
orbit.loadLib(orbit.math.lux)
```

Then call functions through the library namespace:

```orbit
lux.loadLib(orbit.math.lux)

var distance = math.sqrt(81)
signal(distance)
```

The library identifier is written without quotation marks for built-in libraries.

## List available libraries

```powershell
lux list
```

## Inspect a library

```powershell
lux inspect orbit.math
```

## Validate a library manifest

```powershell
lux validate orbit.math
```

## Built-in library overview

The SDK contains these 22 built-in library manifests:

| Library | Namespace | Main purpose |
|---|---|---|
| `orbit.animation` | `animation` | Keyframes, easing, interpolation |
| `orbit.audio` | `audio` | WAV generation, WAV metadata, optional playback |
| `orbit.console` | `console` | Console output and formatting |
| `orbit.data` | `data` | JSON and data helpers |
| `orbit.database` | `database` | SQLite database access |
| `orbit.debug` | `debug` | Assertions, inspection, profiling helpers |
| `orbit.desktop` | `window` / `ui` | Desktop windows and widgets |
| `orbit.files` | `files` | File and directory operations |
| `orbit.fluxengine` | `flux` | Interactive 2D indie-game engine |
| `orbit.graphics` | `graphics` | 2D software drawing and PNG output |
| `orbit.graphics3d` | `graphics3d` | CPU-rendered low-poly 3D helpers |
| `orbit.input` | `input` | Keyboard and mouse state bridge |
| `orbit.math` | `math` | Math and vector helpers |
| `orbit.network` | `network` | HTTP/HTTPS requests and downloads |
| `orbit.office` | `office` | Documents, spreadsheets, presentations, basic PDF export |
| `orbit.physics` | `physics` | Basic 2D physics helpers |
| `orbit.security` | `security` | Hashing and secure token helpers |
| `orbit.system` | `system` | OS, architecture, environment, and process information |
| `orbit.text` | `text` | String and text processing |
| `orbit.time` | `time` | Time and date helpers |
| `orbit.ui` | `ui` | Desktop widgets |
| `orbit.window` | `window` | Desktop window management |

Some desktop-related manifests share the same Tkinter-backed host functionality. See `docs/STANDARD_LIBRARIES.md` for each library's API list and examples.

## Example: math and text

```orbit
lux.loadLib(orbit.math.lux)
lux.loadLib(orbit.text.lux)

signal(math.sqrt(81))
signal(text.upper("hello orbit"))
```

## Example: JSON data

```orbit
lux.loadLib(orbit.data.lux)

var profile = data.json_parse("{\"name\":\"Orbit\",\"level\":4}")
profile["level"] = profile["level"] + 1

signal(data.json_stringify(profile, true))
```

## Example: files

```orbit
lux.loadLib(orbit.files.lux)

files.mkdir("save")
files.write_text("save/player.txt", "Explorer\nLevel 1")

var savedText = files.read_text("save/player.txt")
signal(savedText)
```

## Example: SQLite

```orbit
lux.loadLib(orbit.database.lux)

var db = database.open("build/game.db")
database.execute(db, "CREATE TABLE IF NOT EXISTS scores (name TEXT, points INTEGER)")
database.execute(db, "INSERT INTO scores (name, points) VALUES (?, ?)", ["Explorer", 120])

var rows = database.query(db, "SELECT name, points FROM scores")
signal(rows)
database.close(db)
```

Use parameterized SQL values (`?`) for data rather than joining untrusted text into SQL statements.

## Example: networking

```orbit
lux.loadLib(orbit.network.lux)

// Requires an internet connection.
var response = network.get("https://example.com", 10)
signal(response["status"])
```

Network requests access the network available to the host computer. Only request data from services you are allowed to use.

---

# 14. Create a project

The unified `orbit` command can create starter projects from templates.

## Create a console application

```powershell
orbit new MyConsoleApp --template console-app
cd MyConsoleApp
```

On Linux/macOS:

```sh
orbit new MyConsoleApp --template console-app
cd MyConsoleApp
```

The template contains:

```text
MyConsoleApp/
├── orbit.json
└── src/
    └── main.orb
```

Run the project:

```powershell
orbit run .\src\main.orb
```

## Create a game project

```powershell
orbit new MyGame --template game-prototype
cd MyGame
```

Run the game source:

```powershell
orbit run .\src\main.orb
```

The game template demonstrates a Flux Engine collecting game. It uses Arrow keys or WASD to move, collects four crystals, and exits with Escape or the window close button.

## What is `orbit.json`?

`orbit.json` is the project manifest. It stores application metadata such as its name, version, entry source, and project kind.

A template manifest looks similar to:

```json
{
  "name": "my-console-app",
  "version": "1.0.0",
  "entry": "src/main.orb",
  "kind": "console"
}
```

Keep the manifest in the project root. If you move or rename the entry source file, update the `entry` value to match.

---

# 15. Build and inspect an `.orbit` package

An `.orbit` file is the packaged form of an application.

From the project root, run:

```powershell
orbitpkg build .
```

To specify the output path:

```powershell
orbitpkg build . -o .\build\MyConsoleApp.orbit
```

Run the package using:

```powershell
orbitpkg run .\build\MyConsoleApp.orbit
```

Inspect the package:

```powershell
orbitpkg inspect .\build\MyConsoleApp.orbit
```

The package manager reads the project manifest and entry source, compiles the source, resolves declared Lux dependencies, and writes the app archive.

## Suggested project layout

```text
MyGame/
├── orbit.json
├── src/
│   ├── main.orb
│   └── player.orb
├── assets/
│   ├── images/
│   ├── audio/
│   └── fonts/
├── lib/
│   └── custom-library.lux
├── content/
└── build/
```

This is a suggested organization layout. Keep the `orbit.json` entry path and your actual source files in sync.

- `src/` contains Orbit source.
- `assets/` is for project resources.
- `lib/` is for app-local Lux library manifests.
- `content/` can hold optional project content.
- `build/` is for generated output.

The package builder includes project files and directories in the application archive according to the package builder's rules. Inspect the generated package if you need to verify its exact contents.

---

# 16. Orbit Nano: distribute apps without the full SDK

Orbit Nano is intended to let end users run an Orbit application without installing the complete developer SDK.

Developers still use the full SDK to create and package their applications. Nano is the smaller runtime distribution path.

## Request Nano embedding in source

Add this directive to your main Orbit source:

```orbit
orbit.LoadNano()
```

For example:

```orbit
orbit.LoadNano()

signal("Hello from my packaged Orbit app!")
```

`orbit.LoadNano()` is a compiler/package directive. It is not an instruction to download or install the SDK while the app is running.

## Build the app package

From the project folder:

```powershell
orbitpkg build . -o .\build\MyApp.orbit
```

## Build the Nano player bundle

```powershell
orbitnano build .\build\MyApp.orbit -o .\build\MyApp-Nano.zip
```

The Nano builder creates a compact player bundle for the packaged application, including the runtime components and built-in library dependencies needed by the app.

## Extract the Nano bundle

```powershell
Expand-Archive -LiteralPath .\build\MyApp-Nano.zip -DestinationPath .\build\MyApp-Nano -Force
```

Inspect the extracted folder:

```powershell
Get-ChildItem .\build\MyApp-Nano -Recurse
```

The SDK's Nano distribution instructions and the generated bundle identify the player entry point. Follow the generated bundle's own README or runner file to launch it. Do not assume every future Nano bundle has the same internal file layout.

## Why use Nano?

- End users do not need the full compiler and development tools.
- The player bundle can include only the built-in runtime pieces the app needs.
- Developers can distribute a game or app separately from the full SDK.

Nano does not turn Orbit bytecode into native machine code. It is a compact runtime/player distribution.

For the package format and runtime metadata details, see `docs/PACKAGES.md`.

---

# 17. Build a desktop application

Orbit's desktop helpers use Python Tkinter. Tkinter must be available in the Python installation, and the program must run in a graphical desktop session.

Create a source file such as `notes.orb`:

```orbit
lux.loadLib(orbit.desktop.lux)

var app = window.create("Orbit Notes", 640, 420)
window.label(app, "Hello from Orbit", 24, 24)

var title = ui.entry(app, 24, 64, 280)
ui.text_set(title, "My first note")

window.run(app)
```

Run it:

```powershell
orbit run .\notes.orb
```

The example creates a window, adds a label and text entry, sets initial text, and starts the window event loop.

If you get an error about Tkinter or a display, check the optional requirements section. A headless terminal session cannot display a native desktop window.

The current desktop layer is a small toolkit adapter, not a complete UI framework. See `docs/DESKTOP.md`.

---

# 18. Build a 2D game with Flux Engine

Flux Engine is Orbit's built-in practical 2D indie-game engine.

It provides:

- Scenes and entities.
- Rectangles, circles, and other basic shapes.
- Keyboard movement using Arrow keys or WASD.
- Entity velocity and updates.
- Collectibles and scoring.
- Goal conditions and win messages.
- HUD labels.
- A real-time game window and frame loop.

## Your first playable game

Create `main.orb`:

```orbit
lux.loadLib(orbit.fluxengine.lux)

var game = flux.scene("Crystal Hunt", "#17243b")

// The floor
flux.entity(game, "Meadow", 0, 280, 640, 80, "#355b43", "rect")

// The player
var player = flux.entity(
    game,
    "Player",
    60,
    160,
    34,
    48,
    "#ffcc66",
    "rect"
)

// Enable keyboard movement
flux.control(game, player, 220, "arrows_wasd")
flux.set_bounds(game, true)

// Add items to collect
flux.collectible(game, "Gem A", 220, 120, 22, "#65e8ff")
flux.collectible(game, "Gem B", 480, 220, 22, "#c4b5fd")

// Win after collecting both gems
flux.goal(game, 2, "All gems collected!")

// HUD text
flux.label(game, "CRYSTAL HUNT", 16, 14, "#ffffff", 3)
flux.label(game, "Move with Arrow keys or WASD. Escape quits.", 16, 42, "#c7d8f5", 1)

// Open the actual game window and run the game loop
flux.run(game, 640, 360, "Crystal Hunt", 60)
```

Run it:

```powershell
orbit run .\main.orb
```

## Understand the game functions

| Function | What it does |
|---|---|
| `flux.scene(name, background)` | Creates a scene |
| `flux.entity(...)` | Adds a visible scene entity |
| `flux.control(scene, player, speed, scheme)` | Enables keyboard movement |
| `flux.set_bounds(scene, enabled)` | Keeps the controlled player inside the window |
| `flux.collectible(...)` | Adds a collectible |
| `flux.goal(scene, count, message)` | Sets the number of items required to win |
| `flux.label(...)` | Adds HUD text |
| `flux.velocity(entity, vx, vy)` | Sets entity velocity |
| `flux.update(scene, dt, gravity)` | Advances a scene manually |
| `flux.find(...)` | Finds a scene entity |
| `flux.remove(...)` | Removes an entity |
| `flux.collide(...)` | Checks an axis-aligned collision |
| `flux.render(scene, canvas)` | Draws one still frame to an off-screen canvas |
| `flux.run(scene, width, height, title, fps)` | Opens the game window and starts the live loop |

## Important: `flux.run` versus `flux.render`

These functions do different jobs.

**For a playable game:**

```orbit
flux.run(game, 640, 360, "My Game", 60)
```

This opens a window and continuously processes input, updates the scene, checks supported collisions, and draws frames.

**For a still image:**

```orbit
var frame = graphics.canvas(640, 360, "#17243b")
flux.render(game, frame)
graphics.save_png(frame, "build/scene.png")
```

This draws one frame into an off-screen canvas and saves an image. It does not start a game window or a continuous game loop.

## Controls

For a controlled entity, use one of these schemes:

- `"arrows"`
- `"wasd"`
- `"arrows_wasd"`

Escape or the window close button exits the interactive game.

## Display requirements

Interactive Flux Engine games need Tkinter and a graphical desktop session. A headless machine can still use the off-screen software renderer for images, but cannot show a native game window.

See `docs/FLUX_ENGINE.md` for more details and current engine limitations.

---

# 19. Graphics and simple 3D

## 2D software graphics

Orbit's `graphics` library provides a CPU-based drawing canvas.

Example:

```orbit
lux.loadLib(orbit.graphics.lux)

var canvas = graphics.canvas(640, 360, "#17243b")

graphics.rect(canvas, 40, 50, 180, 100, "#65e8ff")
graphics.circle(canvas, 320, 160, 48, "#ffcc66")
graphics.line(canvas, 20, 300, 620, 300, "#ffffff")
graphics.text(canvas, 24, 20, "ORBIT GRAPHICS", "#ffffff", 2)

graphics.save_png(canvas, "build/graphics-demo.png")
signal("Saved build/graphics-demo.png")
```

This creates a PNG image. It is useful for thumbnails, generated art, test output, and offline rendering.

## Simple 3D rendering

Orbit's `graphics3d` library provides CPU-based low-poly mesh helpers. It can create cube meshes, transform vertices, project triangles, draw flat-shaded surfaces, and draw wireframes.

Example:

```orbit
lux.loadLib(orbit.graphics.lux)
lux.loadLib(orbit.graphics3d.lux)

var frame = graphics.canvas(480, 320, "#101827")
var model = graphics3d.cube(2)

var rotated = graphics3d.transform(
    model,
    [0.45, 0.65, 0.15],
    [0, 0, 0]
)

graphics3d.render_solid(frame, rotated, "#72d7ff", 5, 115)
graphics3d.render_wireframe(frame, rotated, "#ffffff", 5, 115)
graphics.text(frame, 12, 12, "ORBIT LOW-POLY 3D", "#ffffff", 1)
graphics.save_png(frame, "build/orbit-3d.png")

signal("Wrote build/orbit-3d.png")
```

This example renders a still image. It is not a real-time 3D game loop or a GPU shader engine. The 3D renderer is intended for simple low-poly scenes and graphics experiments.

---

# 20. Audio, data, files, networking, and office tools

This section gives short examples of other included libraries. Consult `docs/STANDARD_LIBRARIES.md` for the full documented function list and exact arguments.

## Generate a WAV tone

```orbit
lux.loadLib(orbit.audio.lux)

audio.tone("build/tone.wav", 440, 1.0, 0.5, 44100, "sine")
signal("Created build/tone.wav")
```

The audio library can write WAV files and inspect WAV metadata. Playback delegates to the host operating system's available playback support. It does not currently provide a real-time multi-channel audio mixer or spatial audio engine.

## JSON and data

```orbit
lux.loadLib(orbit.data.lux)

var textData = "{\"name\":\"Orbit\",\"level\":4}"
var data = data.json_parse(textData)

data["level"] = data["level"] + 1

signal(data.json_stringify(data, true))
```

## File operations

```orbit
lux.loadLib(orbit.files.lux)

files.mkdir("build/saves")
files.write_text("build/saves/player.txt", "Explorer")
var savedName = files.read_text("build/saves/player.txt")

signal(savedName)
```

## Database

The database library uses SQLite through Python's standard library:

```orbit
lux.loadLib(orbit.database.lux)

var db = database.open("build/example.db")
database.execute(db, "CREATE TABLE IF NOT EXISTS items (name TEXT, amount INTEGER)")
database.execute(db, "INSERT INTO items (name, amount) VALUES (?, ?)", ["Crystal", 3])

var rows = database.query(db, "SELECT name, amount FROM items")
signal(rows)

database.close(db)
```

## Networking

```orbit
lux.loadLib(orbit.network.lux)

// Requires internet access.
var result = network.get("https://example.com", 10)
signal(result["status"])
```

Keep network requests limited to services you are permitted to access. The runtime applies bounded request/download behavior.

## Office exports

The office library can create simple document, spreadsheet, presentation, and PDF outputs.

```orbit
lux.loadLib(orbit.office.lux)

var doc = office.document("Orbit Report", "Orbit Developer")
office.heading(doc, "Project Summary", 1)
office.paragraph(doc, "This report was created with Orbit.")
office.export_html(doc, "build/report.html")
```

The office tools are lightweight exporters, not full Microsoft Office file compatibility. They do not implement DOCX/XLSX/PPTX import/export or advanced office features.

---

# 21. Orbit Memory Code

Orbit retains a compact legacy encoding called **Orbit Memory Code**.

Its custom base-16 alphabet is:

```text
01234567ABCDEFGH
```

The SDK provides memory encoding commands through the unified `orbit` tool.

Examples:

```powershell
orbit memory encode "HELLO"
orbit memory decode 48454C4C4F
orbit memory address 4096
```

Memory Code has limits in the current implementation:

- A memory character code is limited to values from 0 through 255.
- Memory addresses are limited to 16-bit values (0 through 65535).
- Text encoding supports a restricted character range rather than every Unicode character.

Treat this feature as a compact legacy encoding utility, not a replacement for UTF-8 or a general-purpose storage format.

---

# 22. Format, test, and debug your code

## Format an Orbit source file

```powershell
orbit fmt .\src\main.orb
```

Check formatting without modifying the file:

```powershell
orbit fmt .\src\main.orb --check
```

The formatter currently performs basic whitespace and final-newline normalization. It is not a full code-style formatter.

## Run the SDK tests

From the SDK root on Windows:

```powershell
.\tests\run-tests.ps1
```

On Linux/macOS:

```sh
sh ./tests/run-tests.sh
```

The tests validate the SDK's supported behavior. Some features, such as live network requests and interactive windows, are not suitable for unattended offline tests.

## Inspect BEAM instructions

The debugger can display instructions from a compiled bytecode file:

```powershell
python .\tools\debugger.py .\examples\hello.beam
```

Use the debugger's help if you need more options:

```powershell
python .\tools\debugger.py --help
```

## Profiler

The SDK includes a profiler utility:

```powershell
python .\tools\profiler.py --help
```

Use the help output to see the options supported by the installed version.

## Compiler diagnostics

If compilation fails, read the full message. Orbit errors may identify a line and column. Start by checking:

- The source file path.
- Missing quotes or parentheses.
- Missing braces.
- Misspelled variable or function names.
- Unsupported language syntax.
- Library imports and their manifest names.

---

# 23. Command-line reference

The following are the main commands documented by the SDK.

## Unified `orbit` command

| Command | Purpose |
|---|---|
| `orbit --version` | Print the SDK version |
| `orbit new NAME --template console-app` | Create a console project |
| `orbit new NAME --template game-prototype` | Create a game project |
| `orbit run file.orb` | Compile and execute source |
| `orbit compile file.orb` | Compile source to BEAM |
| `orbit vm file.beam` | Execute BEAM bytecode |
| `orbit package build [project]` | Build an `.orbit` package |
| `orbit package run app.orbit` | Run a packaged application |
| `orbit package inspect app.orbit` | Inspect a package |
| `orbit fmt file.orb` | Normalize basic source formatting |
| `orbit fmt file.orb --check` | Check formatting without modifying |
| `orbit test` | Run the SDK tests |
| `orbit memory encode TEXT` | Encode text using Memory Code |
| `orbit memory decode CODE` | Decode Memory Code |
| `orbit memory address N` | Encode a 16-bit address |

## Individual tools

| Tool | Example | Purpose |
|---|---|---|
| `orbitc` | `orbitc .\hello.orb` | Compile `.orb` to `.beam` |
| `orbitvm` | `orbitvm .\hello.beam` | Run bytecode |
| `orbitpkg` | `orbitpkg build .` | Build, run, or inspect `.orbit` packages |
| `orbitnano` | `orbitnano build .\build\App.orbit -o .\build\App-Nano.zip` | Build Nano player bundle |
| `lux` | `lux list` | Manage and inspect Lux manifests |

For any tool, request its help:

```powershell
orbit --help
orbitc --help
orbitvm --help
orbitpkg --help
orbitnano --help
lux --help
```

---

# 24. Project folder reference

A typical Orbit project can be organized like this:

```text
MyOrbitProject/
├── orbit.json
├── src/
│   ├── main.orb
│   └── helpers.orb
├── assets/
│   ├── images/
│   ├── audio/
│   └── fonts/
├── lib/
│   └── custom-library.lux
├── content/
└── build/
```

| Path | Use |
|---|---|
| `orbit.json` | Project metadata and entry-point declaration |
| `src/` | Orbit source code |
| `assets/` | Images, audio, fonts, and other application resources |
| `lib/` | Application-local Lux library manifests |
| `content/` | Optional content organized by the application developer |
| `build/` | Generated bytecode, packages, exports, and other build artifacts |

This is a recommended organization, not a requirement that every project must contain every folder.

Do not put generated output in place of your source code. Keep source and assets so that you can rebuild the app later.

---

# 25. PowerShell and shell scripts

You can automate common Orbit tasks with small scripts.

## Windows: compile and run a source file

Save this as `Run-Orbit.ps1`:

```powershell
[CmdletBinding()]
param(
    [Parameter(Mandatory = $true)]
    [string]$Source
)

$ErrorActionPreference = 'Stop'

if (-not (Test-Path -LiteralPath $Source -PathType Leaf)) {
    throw "Orbit source file not found: $Source"
}

$command = Get-Command orbit -ErrorAction Stop
& $command.Source run $Source

if ($LASTEXITCODE -ne 0) {
    throw "Orbit exited with code $LASTEXITCODE."
}
```

Run it:

```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
.\Run-Orbit.ps1 -Source .\src\main.orb
```

## Windows: build an application package

Save as `Build-OrbitApp.ps1`:

```powershell
[CmdletBinding()]
param(
    [string]$ProjectPath = ".",
    [string]$OutputPath = ".\build\App.orbit"
)

$ErrorActionPreference = 'Stop'

$project = [System.IO.Path]::GetFullPath($ProjectPath)
$output = [System.IO.Path]::GetFullPath($OutputPath)

if (-not (Test-Path -LiteralPath $project -PathType Container)) {
    throw "Project directory not found: $project"
}

$outputDirectory = Split-Path -Parent $output
New-Item -ItemType Directory -Path $outputDirectory -Force | Out-Null

$command = Get-Command orbitpkg -ErrorAction Stop
& $command.Source build $project -o $output

if ($LASTEXITCODE -ne 0) {
    throw "Orbit package build failed with exit code $LASTEXITCODE."
}

Write-Host "Package created: $output" -ForegroundColor Green
```

Run it from the project root:

```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
.\Build-OrbitApp.ps1 -ProjectPath . -OutputPath .\build\MyApp.orbit
```

## Linux/macOS: run Orbit source

Save as `run-orbit.sh`:

```sh
#!/usr/bin/env sh
set -eu

if [ "$#" -ne 1 ]; then
    echo "Usage: ./run-orbit.sh path/to/program.orb" >&2
    exit 2
fi

orbit run "$1"
```

Make it executable and run it:

```sh
chmod +x ./run-orbit.sh
./run-orbit.sh ./src/main.orb
```

These are optional examples. You can always run the Orbit commands directly without creating helper scripts.

---

# 26. Troubleshooting

## `python` is not recognized

Python is missing or is not on PATH.

1. Install Python 3.10 or newer.
2. Enable the option to add Python to PATH if the installer provides it.
3. Open a new terminal.
4. Run:

```powershell
python --version
```

On some Linux/macOS systems, use `python3 --version`.

## The installer says Python 3.10+ is required

Check the version:

```powershell
python --version
```

Install a supported Python version if it is older than 3.10.

## `orbit` or `orbitc` is not recognized

The SDK's `bin` folder may not be on PATH, or your terminal may have been opened before installation.

1. Confirm the SDK folder still exists.
2. Confirm that its `bin` folder contains the launcher files.
3. Run `install.ps1` again on Windows or `install.sh` on Linux/macOS.
4. Close and reopen the terminal.
5. Check:

```powershell
Get-Command orbit
Get-Command orbitc
```

## PowerShell says scripts are disabled

Use the temporary process-level setting:

```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
```

Then rerun the installer or helper script in that same PowerShell window.

## `orbit run` cannot find the source file

Check your current folder:

```powershell
Get-Location
Get-ChildItem
```

Move to the folder containing the `.orb` file with `cd`, or provide the correct relative or absolute path.

## A program compiles but fails at runtime

Compilation and execution are separate stages. Run the bytecode directly to see the VM/runtime error:

```powershell
orbitc .\main.orb
orbitvm .\main.beam
```

Read the first error message and any line information carefully.

## A Lux library cannot be found

Check the library list:

```powershell
lux list
```

Inspect the manifest:

```powershell
lux inspect orbit.math
```

Make sure the built-in import uses the expected identifier:

```orbit
lux.loadLib(orbit.math.lux)
```

## A desktop window or Flux game window does not appear

Check that:

- Python includes Tkinter.
- You are running in a graphical desktop session.
- The process is not running in a headless server or terminal-only environment.

On Linux, install the Tkinter package supplied by your distribution if needed.

## Audio file is created but nothing plays

WAV generation and audio playback are separate features. The SDK can generate WAV files without an audio device. Playback also depends on the operating system's audio support and player utility.

## The game example saves a PNG instead of opening a game

`graphics.save_png` saves an image. It does not open a live game window.

Use the Flux Engine entry point:

```orbit
flux.run(game, 640, 360, "My Game", 60)
```

Use `flux.render` only when you specifically want to draw a still frame into a graphics canvas.

## Package command fails

Check that the project root contains a valid `orbit.json` and that the manifest's `entry` points to an existing `.orb` file.

Inspect the package manager's available options:

```powershell
orbitpkg --help
```

## Still stuck?

When requesting help, include:

1. Your operating system.
2. The output of `python --version`.
3. The output of `orbit --version`.
4. The exact command you ran.
5. The complete error output.
6. The relevant `.orb` source and `orbit.json` if the problem is project-specific.

Remove passwords, API keys, private tokens, or other secrets before sharing logs or source files.

---

# 27. Known limitations

Orbit 1.3.0 is a practical SDK with a growing set of features. It is important to distinguish implemented features from future possibilities.

The current SDK does **not** advertise the following as implemented:

- Native machine-code compilation.
- Static type checking.
- An optimizing compiler.
- A garbage collector.
- Native FFI.
- An asynchronous scheduler.
- Classes and inheritance.
- A visual IDE/editor.
- A hosted package registry.
- Cryptographic package signing.
- A production-grade debugger or profiler.

Other feature boundaries:

- Flux Engine is a practical 2D indie-game engine, not a full AAA engine.
- The 3D renderer is a CPU-based low-poly renderer, not a GPU shader pipeline.
- Physics is a basic 2D helper layer, not a complete rigid-body solver.
- Audio can generate WAV files and delegate playback, but does not provide a real-time multi-channel mixer or spatial audio.
- Desktop windows and widgets use Tkinter and require a GUI session.
- Office features are lightweight exporters, not full DOCX/XLSX/PPTX compatibility.
- Network features depend on the host machine's internet connection and access permissions.

These boundaries are intentional documentation of the current SDK version, not a statement that the language cannot grow.

---

# 28. Where to go next

A suggested learning path:

1. Run `examples/hello.orb`.
2. Study `examples/expressions.orb`, `functions.orb`, and `loops.orb`.
3. Read `docs/LANGUAGE.md` to learn the grammar.
4. Try `examples/lux.orb` and explore the Lux library list.
5. Create a console project with `orbit new`.
6. Build and inspect an `.orbit` package.
7. Try the game template and read `docs/FLUX_ENGINE.md`.
8. Try the desktop example if Tkinter is installed.
9. Add `orbit.LoadNano()` and explore the Nano build workflow.
10. Run the SDK test suite before making changes to the SDK itself.

## Documentation index

The SDK includes additional technical guides:

| Document | What it explains |
|---|---|
| `docs/ARCHITECTURE.md` | SDK structure and execution pipeline |
| `docs/INSTALLATION.md` | Installation and app distribution |
| `docs/LANGUAGE.md` | Orbit language syntax and grammar |
| `docs/COMPILER.md` | Compiler behavior and bytecode |
| `docs/RUNTIME.md` | VM and runtime behavior |
| `docs/PACKAGES.md` | `.orbit` packages and Orbit Nano |
| `docs/STANDARD_LIBRARIES.md` | Built-in Lux libraries and APIs |
| `docs/FLUX_ENGINE.md` | 2D game engine guide |
| `docs/DESKTOP.md` | Desktop windows and widgets |
| `docs/OFFICE.md` | Office document/export features |

## Have fun building with Orbit!

Start small, experiment, read the examples, and build up your projects one feature at a time.

**Write Orbit. Build something useful. Make something fun.**
