# Orbit SDK 1.3.0 — Learn Orbit by Building Things

**A beginner-friendly tutorial, language reference, library guide, and practical instruction manual.**

Welcome to Orbit! This README is a hands-on course for people who have never programmed before as well as developers learning the Orbit SDK. You will write small programs, run them, change them, and gradually build command-line tools, desktop utilities, graphics, and indie games.

> **Version:** This guide describes the Orbit SDK 1.3.0 package. Examples are written for the compiler, VM, libraries, and tools included in that release. If an example needs a graphical desktop, network access, or a particular host utility, that requirement is called out.

## Learning path

1. Install Orbit and check that the commands work.
2. Write and run your first `.orb` program.
3. Learn every language statement and the core built-in functions.
4. Learn functions, lists, objects, conditions, and loops.
5. Load Lux libraries and use their APIs.
6. Build a desktop app, a graphics program, and a playable 2D game.
7. Package an app and create an Orbit Nano player bundle.
8. Use the reference sections when you need a particular command or API.

## Contents

- [1. What Orbit is](#1-what-orbit-is)
- [2. Install the SDK](#2-install-the-sdk)
- [3. Your first program](#3-your-first-program)
- [4. How Orbit runs code](#4-how-orbit-runs-code)
- [5. Language tutorial: values and variables](#5-language-tutorial-values-and-variables)
- [6. Operators and expressions](#6-operators-and-expressions)
- [7. Every statement, demonstrated](#7-every-statement-demonstrated)
- [8. Built-in functions, demonstrated](#8-built-in-functions-demonstrated)
- [9. Functions](#9-functions)
- [10. Lists and objects](#10-lists-and-objects)
- [11. Lux libraries](#11-lux-libraries)
- [12. Desktop application tutorial](#12-desktop-application-tutorial)
- [13. Graphics tutorial](#13-graphics-tutorial)
- [14. Flux Engine: make a playable game](#14-flux-engine-make-a-playable-game)
- [15. Simple 3D graphics](#15-simple-3d-graphics)
- [16. Files, data, databases, and networking](#16-files-data-databases-and-networking)
- [17. Office document tutorial](#17-office-document-tutorial)
- [18. Create and package a project](#18-create-and-package-a-project)
- [19. Orbit Nano distribution](#19-orbit-nano-distribution)
- [20. Formatting, testing, and debugging](#20-formatting-testing-and-debugging)
- [21. Command-line reference](#21-command-line-reference)
- [22. Complete built-in library reference](#22-complete-built-in-library-reference)
- [23. File formats and project layout](#23-file-formats-and-project-layout)
- [24. Troubleshooting](#24-troubleshooting)
- [25. Security and current limitations](#25-security-and-current-limitations)
- [26. Suggested practice projects](#26-suggested-practice-projects)

---

# 1. What Orbit is

Orbit is a general-purpose programming language and SDK for everyday applications, utilities, creative software, and indie games. The SDK includes:

- An Orbit source compiler and the Orbit virtual machine (VM).
- The `.beam` bytecode format.
- Lux libraries for console, math, text, data, files, time, system information, security, networking, databases, graphics, physics, audio, animation, desktop UI, office exports, and more.
- Flux Engine, a practical 2D indie-game engine with a real-time game window and loop.
- A CPU-based 3D graphics helper library for simple low-poly scenes.
- A package manager for `.orbit` application archives.
- Orbit Nano, a compact Python runtime bundle for distributing an app without the full developer SDK.

Orbit does **not** compile to native machine code in this release. The compiler emits Orbit bytecode and the VM executes it. Nano is a smaller runtime distribution, not a native executable.

## Orbit file extensions

| Extension | Meaning |
|---|---|
| `.orb` | Human-readable Orbit source code |
| `.beam` | JSON-based Orbit bytecode (`ORBIT-BEAM-2`) |
| `.lux` | Lux library manifest / metadata |
| `.orbit` | ZIP-based application package |

# 2. Install the SDK

## Windows 10/11 — PowerShell

1. Install Python **3.10 or newer** and make sure `python` works in PowerShell. The Orbit installer does not install Python.
2. Extract the SDK ZIP into a permanent folder, for example `C:\Orbit\Orbit-SDK-1.3.0`.
3. Open PowerShell in that extracted folder.
4. Run:

```powershell
Set-ExecutionPolicy -Scope Process Bypass
.\install.ps1
```

5. Close PowerShell, open a new PowerShell window, and verify the installation:

```powershell
python --version
orbit --version
orbitc --version
orbitvm --version
orbitpkg --version
orbitnano --version
lux list
```

The installer adds the SDK's `bin` folder to your user PATH. Opening a new terminal makes the updated PATH available. You can launch Orbit from other working directories after installation.

To remove the PATH entry later, run `uninstall.ps1` from the SDK folder. This does not delete the SDK directory.

## Linux

From the extracted SDK directory:

```bash
python3 --version
chmod +x ./install.sh
./install.sh
```

Open a new shell or load the updated profile. The SDK requires Python 3.10+. On Linux, desktop windows may require installing your distribution's Tkinter package (often named `python3-tk`).

## macOS

Use Python 3.10+, then from the SDK directory:

```bash
chmod +x ./install.sh
./install.sh
```

Open a new terminal. Desktop windows require a Python build with Tkinter. Audio playback uses the host's `afplay` utility where available.

## If a command is not recognized

- Check that you opened a **new** terminal after installing.
- Run `python --version` and confirm Python is 3.10+.
- Confirm the SDK `bin` directory is on PATH.
- As a temporary test, run the launcher by its full path from the SDK folder, for example `./bin/orbit` on Linux/macOS or `bin\orbit.cmd` on Windows.

# 3. Your first program

Create a folder for practice and enter it:

```powershell
mkdir OrbitPractice
cd OrbitPractice
```

Create a file named `hello.orb` with this exact code:

```orbit
signal("Hello, Orbit!")
signal("I just ran my first Orbit program.")
```

Run it:

```powershell
orbit run .\hello.orb
```

Expected output:

```text
Hello, Orbit!
I just ran my first Orbit program.
```

`signal(value)` writes a value to standard output followed by a newline. It is the simplest way to display a result while learning Orbit.

## Try the same program in two steps

Compile the source into bytecode:

```powershell
orbitc .\hello.orb
```

Then execute the generated `.beam` file:

```powershell
orbitvm .\hello.beam
```

The `orbit run` command is convenient because it compiles and executes the source in one step.

# 4. How Orbit runs code

```text
hello.orb  --orbitc-->  hello.beam  --orbitvm-->  program output
```

1. You write source code in a `.orb` file.
2. The compiler tokenizes and parses it.
3. The compiler generates `.beam` bytecode.
4. The Orbit VM executes the bytecode.
5. When your code calls a Lux API, the VM calls the corresponding host implementation.

The `.beam` file is JSON-based bytecode, not a native executable. The full SDK compiler and VM are Python programs and require Python 3.10+.

# 5. Language tutorial: values and variables

Orbit values include `null`, booleans, integers, floating-point numbers, strings, lists, and objects (dictionaries).

## Displaying values with `signal()`

```orbit
signal("A message")
signal(42)
signal(3.14)
signal(true)
signal(null)
```

Use `signal()` when you want a quick output line. `print()` is also a built-in output function.

## Variables: `var` and `let`

`var` and `let` both create mutable variables. A mutable variable can be assigned a new value later.

```orbit
var playerName = "Nova"
let score = 0

signal(playerName)
signal(score)

score = 25
signal(score)
```

## Constants: `const`

A `const` binding cannot be reassigned after it is created.

```orbit
const GAME_TITLE = "Star Meadow"
const STARTING_LIVES = 3

signal(GAME_TITLE)
signal(STARTING_LIVES)
```

Trying to assign a new value to a constant causes a runtime error.

## Comments

Comments are notes for people reading the code. Orbit ignores them when it runs the program.

```orbit
// This is a single-line comment.
# This is also a single-line comment.

signal("Comments do not appear in the output.")
```

Comments begin with `//` or `#` outside a string. Strings can use single or double quotes and Python-style backslash escapes. String interpolation is not part of this release.

# 6. Operators and expressions

## Arithmetic

```orbit
signal(8 + 2)   // addition: 10
signal(8 - 2)   // subtraction: 6
signal(8 * 2)   // multiplication: 16
signal(8 / 2)   // division: 4
signal(8 % 3)   // remainder: 2
```

## Comparisons

```orbit
signal(5 == 5)  // true: equal
signal(5 != 3)  // true: not equal
signal(5 > 3)   // true: greater than
signal(5 >= 5)  // true: greater than or equal
signal(2 < 4)   // true: less than
signal(2 <= 2)  // true: less than or equal
```

## Boolean operators

```orbit
var hasKey = true
var doorOpen = false

signal(hasKey and not doorOpen)
signal(hasKey or doorOpen)
signal(hasKey && doorOpen)
signal(hasKey || doorOpen)
signal(!doorOpen)
```

`and` / `&&` and `or` / `||` short-circuit: the right side is only evaluated when needed.

## Membership with `in`

```orbit
var inventory = ["map", "key", "lantern"]
signal("key" in inventory)
signal("sword" in inventory)
```

## Grouping and precedence

Parentheses make the order explicit. Multiplication, division, and remainder happen before addition and subtraction; comparisons happen after arithmetic; boolean AND happens before boolean OR.

```orbit
var result = (2 + 3) * 4
signal(result) // 20
```

# 7. Every statement, demonstrated

This section demonstrates every statement form supported by the 1.3.0 language grammar.

## Variable declarations: `var`, `let`, `const`

```orbit
var coins = 10
let player = "Mira"
const MAX_COINS = 99

coins += 5
signal(player)
signal(coins)
signal(MAX_COINS)
```

## Expression statements and function calls

A function call can be used as a statement even when you do not store its return value.

```orbit
signal("This function call is a statement.")
```

## `if`

```orbit
var temperature = 22
if temperature > 20 {
    signal("It is warm.")
}
```

## `if` / `else`

```orbit
var hasTicket = false
if hasTicket {
    signal("Welcome!")
} else {
    signal("Please get a ticket first.")
}
```

## `else if`

```orbit
var grade = 84
if grade >= 90 {
    signal("A")
} else if grade >= 80 {
    signal("B")
} else if grade >= 70 {
    signal("C")
} else {
    signal("Keep practicing")
}
```

## `while`

A `while` loop repeats while its condition is true. Make sure something inside the loop eventually changes the condition.

```orbit
var countdown = 3
while countdown > 0 {
    signal(countdown)
    countdown -= 1
}
signal("Go!")
```

## `for` / `in`

A `for` loop visits each item in a list or other supported iterable value.

```orbit
for color in ["red", "green", "blue"] {
    signal(color)
}
```

## `break`

`break` exits the nearest loop immediately.

```orbit
for number in range(1, 10) {
    if number == 4 {
        break
    }
    signal(number)
}
signal("The loop ended at 4.")
```

## `continue`

`continue` skips the rest of the current loop iteration and moves to the next one.

```orbit
for number in range(1, 6) {
    if number == 3 {
        continue
    }
    signal(number)
}
```

## `fn` and `function`

Both keywords define a function. `fn` is the shorter style used throughout this guide.

```orbit
fn greet() {
    signal("Hello from a function!")
}

function sayBye() {
    signal("Goodbye!")
}

greet()
sayBye()
```

## `return`

`return` sends a value back to the code that called the function. A function can also return without a value.

```orbit
fn double(number) {
    return number * 2
}

var answer = double(21)
signal(answer)
```

## `orbit.LoadNano()` build directive

This special zero-argument directive asks the package builder to embed the compact Nano runtime when it builds the application.

```orbit
orbit.LoadNano()
signal("This app requests an embedded Nano runtime.")
```

It emits a `LOAD_NANO` marker. It is a no-op during ordinary source execution; it does not download anything while the program runs. See [Orbit Nano distribution](#19-orbit-nano-distribution).

## Statement separators and blocks

Newlines normally separate statements. A semicolon (`;`) can also separate statements on the same line. Curly braces `{` and `}` delimit blocks.

```orbit
var first = 1; var second = 2
if first < second { signal("first is smaller") }
```

# 8. Built-in functions, demonstrated

These core functions are available without loading a Lux library.

## `signal(value)` and `print(value)`

Write one value to standard output. Each call writes a line.

```orbit
signal("This appears in the terminal.")
print("This also appears in the terminal.")
```

## `len(value)`

Get the length of a string, list, or supported collection.

```orbit
signal(len("Orbit"))       // 5
signal(len([10, 20, 30]))   // 3
```

## `str(value)`

Convert a value to a string.

```orbit
var lives = 3
signal(str(lives))
signal("Lives remaining: " + str(lives))
```

## `int(value)` and `float(value)`

Convert a compatible value to an integer or floating-point number.

```orbit
signal(int("42"))
signal(float("3.5"))
```

## `bool(value)`

Convert a value to a boolean using Orbit's runtime truth rules.

```orbit
signal(bool(1))
signal(bool(0))
signal(bool("hello"))
```

## `type(value)`

Return the runtime type name of a value.

```orbit
signal(type(12))
signal(type("hello"))
signal(type([1, 2]))
signal(type({name: "Orbit"}))
```

## `abs(value)`

Return the absolute value.

```orbit
signal(abs(-17))
```

## `min(...)` and `max(...)`

Return the smallest or largest supplied value.

```orbit
signal(min(8, 2, 5))
signal(max(8, 2, 5))
```

## `round(value)`

Round a numeric value to the nearest integer according to the runtime's rounding behavior.

```orbit
signal(round(3.6))
signal(round(3.2))
```

## `range(...)`

Create a sequence of integers for loops. Supported forms are `range(stop)`, `range(start, stop)`, and `range(start, stop, step)`.

```orbit
for a in range(4) { signal(a) }          // 0, 1, 2, 3
for b in range(2, 5) { signal(b) }       // 2, 3, 4
for c in range(10, 4, -2) { signal(c) }  // 10, 8, 6
```

## A combined built-ins practice script

Save as `builtins.orb` and run `orbit run .\builtins.orb`:

```orbit
var name = "Orbit"
var values = [4, 8, 15, 16, 23, 42]

signal("Name: " + str(name))
signal("Characters: " + str(len(name)))
signal("Values: " + str(len(values)))
signal("Type: " + type(values))
signal("Absolute: " + str(abs(-12)))
signal("Minimum: " + str(min(4, 8, 15)))
signal("Maximum: " + str(max(4, 8, 15)))
signal("Rounded: " + str(round(6.7)))
signal("Integer: " + str(int("123")))
signal("Float: " + str(float("2.5")))
signal("Boolean: " + str(bool(1)))
```

# 9. Functions

Functions let you give a name to reusable code. They can accept positional parameters and return a value.

```orbit
fn greet(name) {
    signal("Hello, " + name + "!")
}

greet("Ari")
greet("Sam")
```

## Returning a result

```orbit
fn multiply(a, b) {
    return a * b
}

var total = multiply(6, 7)
signal(total)
```

## Calling one function from another

```orbit
fn square(number) {
    return number * number
}

fn sum_of_squares(a, b) {
    return square(a) + square(b)
}

signal(sum_of_squares(3, 4))
```

## Recursion

A function can call itself. Always include a stopping condition.

```orbit
fn countdown(number) {
    if number <= 0 {
        signal("Finished")
        return
    }
    signal(number)
    countdown(number - 1)
}

countdown(3)
```

Function parameters and local variables are scoped to the function. A function can read global bindings. This release does not support closures, nested lexical captures, default parameters, named arguments, function values, lambdas, overloads, or nested function definitions.

# 10. Lists and objects

## Lists

Lists hold ordered values. Indexes start at zero.

```orbit
var inventory = ["key", "map", "potion"]
signal(inventory[0])
signal(inventory[2])

inventory[1] = "compass"
inventory[0] = "old key"
signal(inventory)
```

List methods are not required for basic use; the `orbit.data` library also provides collection helpers such as `data.push`, `data.pop`, `data.contains`, and `data.unique`.

## Objects (dictionaries)

Objects store values under keys.

```orbit
var player = {name: "Nova", score: 120, active: true}

signal(player.name)
signal(player["score"])
player.score += 10
player.name = "Nova Star"
signal(player)
```

## Combining lists and objects

```orbit
var firstMember = {name: "Nova", level: 4}
var secondMember = {name: "Pip", level: 2}
var party = [firstMember, secondMember]

signal(party[0].name)
signal(party[1]["level"])
```

## Assignment operators

```orbit
var points = 10
points += 5
points -= 2
points *= 3
points /= 2
signal(points)
```

Indexed and member assignment are supported too, for example `scores[0] = 99` and `player.score += 1`.

# 11. Lux libraries

Lux libraries add capabilities beyond the core language. Before using a library, load its built-in manifest:

```orbit
lux.loadLib(orbit.math.lux)
```

Then call its functions through the namespace:

```orbit
signal(math.sqrt(81))
```

The general pattern is:

```orbit
lux.loadLib(orbit.library_name.lux)
// Call functions using the library's namespace:
// library_name.some_function(...)
```

`lux.loadLib(...)` validates and records a dependency. It is not a security sandbox or a per-function permission system. Built-in library references use `orbit.<name>.lux`. A custom library can be loaded from a project path, for example `lux.loadLib("lib/custom-library.lux")`.

See the [complete built-in library reference](#22-complete-built-in-library-reference) for every documented API in this SDK release.

# 12. Desktop application tutorial

Orbit's desktop helpers use Python Tkinter. You need a graphical desktop session and a Python installation that includes Tkinter. This example opens a real window.

Create `notes.orb`:

```orbit
lux.loadLib(orbit.desktop.lux)

var app = window.create("Orbit Notes", 640, 420)
window.label(app, "Welcome to Orbit Notes", 24, 24)

var title = ui.entry(app, 24, 64, 280)
ui.text_set(title, "My first note")

window.run(app)
```

Run it:

```powershell
orbit run .\notes.orb
```

The program creates a window, adds a label and text-entry widget, fills the entry, and starts the window event loop. The UI library also has buttons and checkboxes; the exact APIs are listed in the reference section. The toolkit is a small Tkinter adapter, not a complete visual IDE or advanced layout framework.

# 13. Graphics tutorial

The `orbit.graphics` library creates an off-screen software canvas. It is useful for drawing images, thumbnails, simple art, and generated assets. It does not automatically open a desktop window.

Create `drawing.orb`:

```orbit
lux.loadLib(orbit.graphics.lux)

var canvas = graphics.canvas(640, 360, "#17243b")
graphics.rect(canvas, 40, 50, 220, 120, "#ffb454", true, 2)
graphics.circle(canvas, 380, 130, 55, "#65d7ff", true, 2)
graphics.line(canvas, 40, 250, 580, 250, "#ffffff", 3)
graphics.text(canvas, 24, 20, "My first Orbit drawing", "#ffffff", 2)
graphics.save_png(canvas, "drawing.png")

signal("Saved drawing.png")
```

Run:

```powershell
orbit run .\drawing.orb
```

Open `drawing.png` in an image viewer. The canvas is limited to 4096×4096 pixels. Colors can be hex strings, common names, or RGB(A) lists. The built-in bitmap font is a small 5×7 font.

## Pixel and shape practice

```orbit
lux.loadLib(orbit.graphics.lux)
var image = graphics.canvas(160, 120, "#101820")
graphics.pixel(image, 10, 10, "#ffffff")
signal(graphics.pixel_get(image, 10, 10))
graphics.polygon(image, [[80, 10], [140, 100], [20, 100]], "#50d890", true, 1)
graphics.save_png(image, "shapes.png")
```

# 14. Flux Engine: make a playable game

Flux Engine is Orbit's practical 2D indie-game engine. Unlike a one-frame graphics export, `flux.run(...)` opens a game window and runs a continuous update/input/render loop.

Create `game.orb`:

```orbit
lux.loadLib(orbit.fluxengine.lux)

var game = flux.scene("Crystal Hunt", "#17243b")
flux.entity(game, "Meadow", 0, 280, 640, 80, "#355b43", "rect")
var player = flux.entity(game, "Player", 60, 160, 34, 48, "#ffcc66", "rect")
flux.control(game, player, 220, "arrows_wasd")
flux.collectible(game, "Gem A", 220, 120, 22, "#65e8ff")
flux.collectible(game, "Gem B", 480, 220, 22, "#c4b5fd")
flux.goal(game, 2, "All gems collected!")
flux.label(game, "Find every crystal", 16, 16, "#ffffff", 2)
flux.run(game, 640, 360, "Crystal Hunt", 60)
```

Run:

```powershell
orbit run .\game.orb
```

Use **WASD** or the **arrow keys** to move. Collect the gems to reach the goal. Press Escape or close the window to exit.

## What each line does

- `flux.scene(...)` creates a scene and sets its background.
- `flux.entity(...)` creates a visible entity.
- `flux.control(...)` makes the player respond to keyboard input.
- `flux.collectible(...)` adds a collectible object.
- `flux.goal(...)` sets the number of collectibles needed to win and the win message.
- `flux.label(...)` adds a simple HUD label.
- `flux.run(...)` opens the actual game window and starts the loop.

The supported control schemes are `arrows`, `wasd`, and `arrows_wasd`. Flux supports rectangles, circles, lines, and text entities. The live game window requires Tkinter and a graphical desktop session.

## Render a still screenshot instead

`flux.render` draws one frame into a graphics canvas. It does not launch the interactive game loop.

```orbit
lux.loadLib(orbit.fluxengine.lux)
lux.loadLib(orbit.graphics.lux)

var scene = flux.scene("Screenshot", "#17243b")
flux.entity(scene, "Player", 40, 50, 32, 48, "#ffcc66", "rect")
var canvas = graphics.canvas(320, 180, "#17243b")
flux.render(scene, canvas)
graphics.save_png(canvas, "scene.png")
```

Use `flux.run` for a playable game and `flux.render` for a still image or headless rendering task.

# 15. Simple 3D graphics

The `orbit.graphics3d` library supplies CPU-based mesh transformation, projection, flat-shaded faces, and wireframe rendering. It is useful for simple low-poly experiments, not a GPU shader pipeline.

```orbit
lux.loadLib(orbit.graphics.lux)
lux.loadLib(orbit.graphics3d.lux)

var canvas = graphics.canvas(480, 320, "#101827")
var cube = graphics3d.transform(graphics3d.cube(1.8), [0.45, 0.65, 0.15], [0, 0, 0])
graphics3d.render_solid(canvas, cube, "#72d7ff", 5, 115)
graphics3d.render_wireframe(canvas, cube, "#ffffff", 5, 115)
graphics.save_png(canvas, "cube.png")
```

Run the script and open `cube.png`. See the library reference for mesh creation, transforms, projection, and rendering functions.

# 16. Files, data, databases, and networking

## Read and write a text file

```orbit
lux.loadLib(orbit.files.lux)

files.mkdir("build")
files.write_text("build/hello.txt", "Saved by Orbit!\n")
var contents = files.read_text("build/hello.txt")
signal(contents)
signal(files.exists("build/hello.txt"))
```

Relative paths resolve from the program's working directory. Orbit does not sandbox filesystem access.

## JSON data

```orbit
lux.loadLib(orbit.data.lux)

var source = "{\"name\":\"Orbit\",\"version\":1}"
var record = data.json_parse(source)
record["version"] += 1
var output = data.json_stringify(record, true)
signal(output)
```

## SQLite database

```orbit
lux.loadLib(orbit.database.lux)

var db = database.open_database(":memory:")
database.execute(db, "CREATE TABLE notes (body TEXT)", [])
database.execute(db, "INSERT INTO notes (body) VALUES (?)", ["Hello from Orbit"])
var rows = database.query(db, "SELECT body FROM notes", [])
signal(rows[0]["body"])
database.close(db)
```

Use `?` placeholders and a parameter list for values. Do not build SQL statements by concatenating untrusted input.

## HTTP GET

```orbit
lux.loadLib(orbit.network.lux)
var response = network.get("https://example.com", 10)
signal(response)
```

Networking requires internet access. Only absolute HTTP/HTTPS URLs are accepted, and responses are capped at 2 MiB. Do not send private credentials to websites you do not trust. See the API reference for POST, HEAD, and download calls.

# 17. Office document tutorial

The `orbit.office` library creates simple document, spreadsheet, presentation, and PDF outputs. It is not a replacement for full Microsoft Office file compatibility.

```orbit
lux.loadLib(orbit.office.lux)

var report = office.document("Orbit Report", "Orbit Learner")
office.heading(report, "My First Report", 1)
office.paragraph(report, "This document was generated with Orbit.")
office.export_html(report, "report.html")

var sheet = office.sheet(10, 4)
office.cell_set(sheet, "A1", 12)
office.cell_set(sheet, "A2", 30)
office.cell_set(sheet, "A3", "=SUM(A1:A2)")
signal(office.cell_get(sheet, "A3"))
office.export_csv(sheet, "totals.csv")

var slides = office.presentation("Orbit Slides")
office.slide_add(slides, "Welcome", "This is my first slide.")
office.export_presentation_html(slides, "slides.html")

office.export_pdf_text("Orbit PDF", "A simple PDF created by Orbit.", "summary.pdf")
```

# 18. Create and package a project

A project keeps source code, assets, custom libraries, and build output organized.

## Create a starter project

```powershell
orbit new MyConsoleApp --template console-app
```

Or create a game starter:

```powershell
orbit new MyGame --template game-prototype
```

Enter the project folder and inspect the files:

```powershell
cd .\MyConsoleApp
Get-ChildItem -Recurse
```

A typical project contains an `orbit.json` manifest and an entry source file. Always check that the manifest's `entry` path matches the actual `.orb` file.

## Example project layout

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
└── build/
```

This is a suggested organization; keep your manifest and real paths consistent. `assets/` is where project resources belong. Keep nested folders and filenames intact. App-local custom Lux libraries belong in `lib/`.

## Build a package

From the project root:

```powershell
orbitpkg build . -o .\build\MyGame.orbit
```

Inspect the package:

```powershell
orbitpkg inspect .\build\MyGame.orbit
```

Run it:

```powershell
orbitpkg run .\build\MyGame.orbit
```

The `.orbit` file is a ZIP-based package. It contains the compiled entry bytecode, manifest, project assets, and app-local library files according to the package builder's rules.

# 19. Orbit Nano distribution

Orbit Nano is a compact runtime bundle for users who need to run an Orbit app but do not need the full developer SDK.

## Step 1: Add the build directive

Put this in the application's main `.orb` file:

```orbit
orbit.LoadNano()
signal("Hello from my Orbit application!")
```

## Step 2: Build the `.orbit` package

```powershell
orbitpkg build . -o .\build\MyApp.orbit
```

The package builder detects the `LOAD_NANO` marker and includes Nano runtime metadata and components in the `.orbit` package.

## Step 3: Create the player bundle

```powershell
orbitnano build .\build\MyApp.orbit -o .\build\MyApp-Nano.zip
```

## Step 4: Extract and run the player bundle

```powershell
Expand-Archive -LiteralPath .\build\MyApp-Nano.zip -DestinationPath .\build\MyApp-Nano -Force
cd .\build\MyApp-Nano
python run.py
```

The generated bundle may also include `run.ps1` or `run.bat`. The bundle's own runner/README is the authority for its launch instructions.

**Important:** Orbit Nano in 1.3.0 is a compact Python source-runtime bundle. It requires Python 3.10+ on the player's device. It does not include the compiler and is not a native single-file executable. Built-in runtime modules are selected according to the app's dependencies. App-local custom Lux files remain inside the app package's `lib/` directory.

# 20. Formatting, testing, and debugging

## Format a source file

```powershell
orbit fmt .\main.orb
```

Check formatting without changing the file:

```powershell
orbit fmt .\main.orb --check
```

## Run the SDK tests

From the SDK root:

```powershell
.\tests\run-tests.ps1
```

On Linux/macOS:

```bash
bash ./tests/run-tests.sh
```

## Inspect command help

```powershell
orbit --help
orbitc --help
orbitvm --help
orbitpkg --help
orbitnano --help
lux --help
```

The SDK includes helper debugger and profiler tools. Their available options can change; use each tool's `--help` output before relying on a particular option.

# 21. Command-line reference

| Command | Purpose |
|---|---|
| `orbit --version` | Show Orbit CLI version |
| `orbit run file.orb` | Compile and run a source file |
| `orbit new Name --template console-app` | Create a console starter project |
| `orbit new Name --template game-prototype` | Create a Flux game starter project |
| `orbitc file.orb` | Compile source to `.beam` |
| `orbitvm file.beam` | Run bytecode |
| `orbitpkg build .` | Build an `.orbit` application package |
| `orbitpkg inspect app.orbit` | Inspect a package |
| `orbitpkg run app.orbit` | Run a package |
| `orbitnano build app.orbit -o player.zip` | Build a Nano player bundle |
| `lux list` | List built-in Lux libraries |
| `lux inspect orbit.math` | Inspect a Lux library |
| `lux validate orbit.math` | Validate a Lux manifest |
| `orbit fmt file.orb` | Format source code |
| `orbit test` | Run the SDK test command, if invoked from an SDK installation |
| `orbit memory encode TEXT` | Encode text using Orbit Memory Code |
| `orbit memory decode CODE` | Decode Orbit Memory Code |
| `orbit memory address N` | Convert an address value |

For exact flags in your installed build, run the command with `--help`.

# 22. Complete built-in library reference

Load each built-in library before calling its namespace, for example `lux.loadLib(orbit.math.lux)`. The signatures below summarize the documented 1.3.0 APIs. Host requirements and limitations are noted.

## Console — `orbit.console`

```orbit
lux.loadLib(orbit.console.lux)
console.write("Text without adding a newline")
console.log("A log line")
var answer = console.read_line("Your name: ")
console.color("Colored terminal text", 32)
console.clear()
signal("You entered: " + answer)
```

APIs: `console.write(value)`, `console.log(value)`, `console.read_line(prompt)`, `console.clear()`, `console.color(value, ansi_code)`.

## Math — `orbit.math`

```orbit
lux.loadLib(orbit.math.lux)
signal(math.sqrt(81))
signal(math.sin(math.radians(90)))
signal(math.clamp(120, 0, 100))
signal(math.distance(math.vec2(0, 0), math.vec2(3, 4)))
signal(math.random_int(1, 6))
```

APIs: `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `atan2`, `sqrt`, `floor`, `ceil`, `log`, `log10`, `exp`; `vec2(x,y)`, `vec3(x,y,z)`, `dot(a,b)`, `length(v)`, `normalize(v)`, `distance(a,b)`, `lerp(a,b,t)`, `clamp(x,min,max)`, `map_range(x,in_min,in_max,out_min,out_max)`, `radians`, `degrees`, `random()`, `random_int(min,max)`, `pi()`, `tau()`, `e()`, `hypot(x,y)`, `is_finite(value)`.

## Text — `orbit.text`

```orbit
lux.loadLib(orbit.text.lux)
var message = "  hello orbit  "
signal(text.upper(text.trim(message)))
signal(text.replace("red blue", "blue", "green"))
signal(text.join(text.split("a,b,c", ","), " | "))
```

APIs: `upper`, `lower`, `trim`, `split`, `join`, `replace`, `contains`, `starts_with`, `ends_with`, `substring`, `format`, `normalize_unicode`, `matches`, `regex_find_all`, `codepoints`, `from_codepoints`.

## Data — `orbit.data`

```orbit
lux.loadLib(orbit.data.lux)
var values = [3, 1, 3, 2]
signal(data.unique(values))
signal(data.sort(values))
data.push(values, 9)
signal(data.pop(values))
signal(data.json_stringify({name: "Orbit"}, true))
```

APIs: `json_parse`, `json_stringify(value, pretty)`, `csv_parse`, `csv_stringify`, Base64 encode/decode helpers, `keys`, `values`, `has`, `merge`, `sort`, `unique`, `push`, `pop`, `contains`.

## Files — `orbit.files`

```orbit
lux.loadLib(orbit.files.lux)
files.mkdir("build")
files.write_text("build/note.txt", "Orbit file demo")
signal(files.read_text("build/note.txt"))
signal(files.extension("build/note.txt"))
signal(files.sha256("build/note.txt"))
```

APIs: `exists`, `is_file`, `is_dir`, `read_text`, `write_text`, `read_bytes`, `write_bytes`, `append_text`, `mkdir`, `list_dir`, `copy`, `move`, `remove`, `join`, `name`, `extension`, `sha256`. Paths are resolved by the host process and are not sandboxed.

## Time — `orbit.time`

```orbit
lux.loadLib(orbit.time.lux)
var start = time.measure_start()
time.sleep(0.1)
signal(time.elapsed(start))
signal(time.date_string())
signal(time.timestamp())
```

APIs: `now()`, `unix()`, `timestamp()`, `date_string()`, `sleep(seconds)`, `measure_start()`, `elapsed(start)`, `format_timestamp(timestamp, format)`.

## System — `orbit.system`

```orbit
lux.loadLib(orbit.system.lux)
signal(system.os_name())
signal(system.architecture())
signal(system.python_version())
signal(system.cwd())
signal(system.env("PATH", "not set"))
```

APIs: `os_name`, `os_version`, `architecture`, `python_version`, `cwd`, `env(name, default)`, `set_env(name,value)`, `args`, `home`, `newline`. Values are supplied by the host environment.

## Security — `orbit.security`

```orbit
lux.loadLib(orbit.security.lux)
var digest = security.sha256("hello")
signal(digest)
signal(security.constant_time_equal(digest, digest))
signal(security.secure_token(24))
```

APIs: `sha256`, `sha512`, `hmac_sha256(key,message)`, `secure_token(length)`, `random_bytes(length)`, `constant_time_equal(a,b)`, and Base64 helpers. These utilities are not a complete application security design or secure secret store.

## Networking — `orbit.network`

```orbit
lux.loadLib(orbit.network.lux)
var response = network.get("https://example.com", 10)
signal(response)
```

APIs: `get(url,timeout)`, `post(url,body,content_type,timeout)`, `head(url,timeout)`, `download(url,path,timeout)`. Only absolute HTTP/HTTPS URLs are accepted. Responses are capped at 2 MiB. There is no async I/O, WebSocket, TCP server, custom TLS configuration, or connection pool in this release.

## Database — `orbit.database`

```orbit
lux.loadLib(orbit.database.lux)
var db = database.open_database(":memory:")
database.execute(db, "CREATE TABLE scores (name TEXT, points INTEGER)", [])
database.execute(db, "INSERT INTO scores VALUES (?, ?)", ["Nova", 42])
var rows = database.query(db, "SELECT name, points FROM scores", [])
signal(rows[0]["name"])
database.close(db)
```

APIs: `open_database(path)`, `execute(db,sql,params)`, `query(db,sql,params)`, `table_exists(db,name)`, `close(db)`. Uses SQLite. Bind values with `?` placeholders.

## Graphics 2D — `orbit.graphics`

```orbit
lux.loadLib(orbit.graphics.lux)
var canvas = graphics.canvas(200, 120, "#18243a")
graphics.rect(canvas, 10, 10, 60, 40, "#ff8800", true, 1)
graphics.circle(canvas, 130, 55, 25, "#58d7ff", true, 1)
graphics.text(canvas, 10, 90, "Orbit", "#ffffff", 2)
graphics.save_png(canvas, "graphics-demo.png")
```

APIs: `canvas(width,height,background)`, `clear`, `pixel`, `pixel_get`, `rect(canvas,x,y,w,h,color,filled,thickness)`, `line`, `circle`, `polygon`, `text`, `save_png(canvas,path)`. Canvas limit: 4096×4096. RGBA software renderer; PNG output uses zlib.

## Graphics 3D — `orbit.graphics3d`

```orbit
lux.loadLib(orbit.graphics.lux)
lux.loadLib(orbit.graphics3d.lux)
var canvas = graphics.canvas(320, 240, "#101827")
var mesh = graphics3d.cube(1.5)
var turned = graphics3d.transform(mesh, [0.3, 0.6, 0], [0, 0, 0])
graphics3d.render_solid(canvas, turned, "#72d7ff", 5, 100)
graphics.save_png(canvas, "cube-demo.png")
```

APIs: `cube(size)`, `mesh(vertices,edges,faces)`, `transform(mesh,rotation,position,scale)`, `project_point(point,width,height,camera_z,scale)`, `render_wireframe(canvas,mesh,color,camera_z,scale)`, `render_solid(canvas,mesh,color,camera_z,scale,light_dir)`. CPU renderer for simple low-poly scenes.

## Flux Engine — `orbit.fluxengine`

```orbit
lux.loadLib(orbit.fluxengine.lux)
var scene = flux.scene("Test", "#17243b")
var player = flux.entity(scene, "Player", 20, 30, 30, 40, "#ffcc66", "rect")
flux.velocity(player, 10, 0)
flux.update(scene, 0.5, 0)
signal(flux.find(scene, "Player"))
```

APIs: `scene(name,background)`, `entity(scene,name,x,y,width,height,color,shape)`, `velocity(entity,vx,vy)`, `control(scene,player,speed,scheme)`, `collectible(scene,name,x,y,size,color)`, `goal(scene,count,message)`, `label(scene,text,x,y,color,size)`, `set_bounds(scene,enabled)`, `update(scene,dt,gravity)`, `render(scene,canvas)`, `find`, `remove`, `collide`, `run(scene,width,height,title,fps)`. `run` opens a live Tk window; `render` draws one off-screen frame.

## Physics — `orbit.physics`

```orbit
lux.loadLib(orbit.physics.lux)
var body = physics.body(0, 0, 3, 0, 1, 2)
physics.apply_force(body, 0, 10)
physics.integrate(body, 0.5, 0, 10)
signal(body)
```

APIs: `body(x,y,vx,vy,mass,radius)`, `apply_force`, `integrate`, `circle_overlap`, `aabb_overlap`, `distance`, `bounce`, `step`, `resolve_aabb`, `resolve_circle`. This is a basic deterministic 2D helper layer, not a full rigid-body engine.

## Animation — `orbit.animation`

```orbit
lux.loadLib(orbit.animation.lux)
var keys = [animation.keyframe(0, 0), animation.keyframe(1, 100)]
signal(animation.timeline(keys, 0.5, "ease_in_out"))
signal(animation.ping_pong(0.75, 1))
```

APIs: `keyframe(time,value)`, `timeline(keyframes,time,easing)`, `lerp`, `ease_in`, `ease_out`, `ease_in_out`, `ping_pong`.

## Audio — `orbit.audio`

```orbit
lux.loadLib(orbit.audio.lux)
audio.tone("notification.wav", 660, 0.2, 0.2, 44100, "sine")
signal(audio.info("notification.wav"))
// Optional playback depends on a host audio utility:
// audio.play("notification.wav")
```

APIs: `tone(path,frequency,duration,volume,sample_rate,waveform)`, `info(path)`, `play(path)`. `tone` writes mono 16-bit PCM WAV; waveforms are `sine`, `square`, `saw`, and `triangle`. Playback uses `winsound` (Windows), `afplay` (macOS), or `aplay` (Linux) when available. There is no real-time multi-channel mixer or spatial audio.

## Desktop windows — `orbit.window`, `orbit.ui`, `orbit.input`, `orbit.desktop`

```orbit
lux.loadLib(orbit.desktop.lux)
var app = window.create("Orbit Window", 420, 260)
window.set_title(app, "Orbit Window Demo")
window.label(app, "A native desktop window", 20, 20)
var field = ui.entry(app, 20, 60, 240)
ui.text_set(field, "Edit this text")
window.run(app)
```

Window APIs: `window.create(title,width,height)`, `window.label`, `window.run`, `window.close`, `window.set_title`, `window.set_size`.

UI APIs: `ui.button`, `ui.entry`, `ui.text_get`, `ui.text_set`, `ui.checkbox`, `ui.checked`.

Input APIs: `input.bind_tk(window)`, `key_down`, `mouse_position`, `mouse_down`.

These APIs need Tkinter and an active graphical desktop for interactive use. Headless-safe input queries may return a neutral state. The toolkit is intentionally small.

## Debug — `orbit.debug`

```orbit
lux.loadLib(orbit.debug.lux)
debug.assert_true(2 + 2 == 4, "Math should work")
signal(debug.type_of("hello"))
signal(debug.inspect({score: 12}))
var started = debug.profile_start()
var elapsed = debug.profile_end(started)
signal(elapsed)
debug.breakpoint_message("Reached the checkpoint")
```

APIs: `assert_true(condition,message)`, `type_of(value)`, `inspect(value)`, `profile_start()`, `profile_end(start)`, `breakpoint_message(message)`.

## Office — `orbit.office`

```orbit
lux.loadLib(orbit.office.lux)
var doc = office.document("Demo", "Orbit")
office.heading(doc, "Heading", 1)
office.paragraph(doc, "A paragraph of text.")
office.export_html(doc, "demo.html")
```

Document APIs: `document(title,author)`, `paragraph`, `heading`, `table`, `export_html(document,path)`.

Spreadsheet APIs: `sheet(rows,columns)`, `cell_set(sheet,cell,value)`, `cell_get(sheet,cell)`, `export_csv(sheet,path)`. Basic arithmetic, cell references, `SUM(A1:B4)`, and `AVERAGE(A1:B4)` formulas are supported.

Presentation APIs: `presentation(title)`, `slide_add(deck,title,body)`, `export_presentation_html(deck,path)`.

PDF API: `export_pdf_text(title,text,path)` creates a minimal one-page text PDF.

This library does not provide full DOCX/XLSX/PPTX import/export compatibility or advanced office-suite features.

# 23. File formats and project layout

| File/folder | Purpose |
|---|---|
| `orbit.json` | Project manifest and entry-point configuration |
| `src/` | Orbit source files (if your project uses this convention) |
| `assets/` | Images, audio, fonts, levels, and other project resources |
| `lib/` | App-local custom Lux manifests, source, and bytecode |
| `content/` | Optional content you organize separately |
| `build/` | Generated packages and output files |
| `main.beam` | Compiled program bytecode in an application package |
| `nano/` | Nano runtime files in a Nano bundle; exact bundle layout is generated by the tool |

The package builder includes files according to its package rules. After building, use `orbitpkg inspect` to verify the archive contents. Do not assume a suggested project layout changes the builder's manifest rules.

# 24. Troubleshooting

## `orbit` is not recognized

Open a new terminal after installation, confirm the SDK `bin` directory is on PATH, and check `python --version`.

## Python version is too old

Install Python 3.10 or newer and make sure the `python` command points to that version.

## `No module named tkinter` or no window appears

Install/enable Tkinter for your Python distribution and run from a graphical desktop session. A headless terminal or server cannot display desktop windows.

## My Flux script only creates a PNG

A script using `graphics.canvas`, `flux.render`, and `graphics.save_png` only renders a still image. Call `flux.run(scene,width,height,title,fps)` to launch the interactive game window.

## Audio file is created but I hear nothing

WAV generation and playback are separate. Install/enable the platform playback utility or call `audio.info` to confirm the WAV file was generated.

## Package build fails

Check that the project root contains `orbit.json`, that the entry path exists, and that all declared Lux libraries can be resolved. Run `orbitpkg --help` for available options.

## How to ask for help

Include your operating system, Python version, Orbit version, exact command, complete error message, and the relevant source/manifest. Remove passwords, tokens, and private information before sharing logs.

# 25. Security and current limitations

Orbit programs and host library functions run with the permissions of the current operating-system user. The VM is **not a security sandbox**. Only run source, bytecode, packages, and Nano bundles you trust. Filesystem and network libraries can access resources available to the host process.

Important 1.3.0 boundaries:

- No native machine-code compiler, static type checker, optimizer, or garbage collector.
- No native FFI, asynchronous scheduler, async/await, coroutines, or concurrency framework.
- No classes, inheritance, structs, generics, enums, exceptions/`try`, switch/match, lambdas, closures, macros, or string interpolation.
- No visual IDE/editor or hosted package registry; no cryptographic package signing.
- Flux is a practical 2D indie-game engine, not a full AAA engine. It has no visual editor, sprite atlas importer, full animation state machine, integrated multiplayer stack, real-time audio mixer, or integrated rigid-body solver.
- `graphics3d` is a CPU-based low-poly renderer, not a GPU shader pipeline.
- Audio generates WAV and delegates playback; it is not a real-time multi-channel mixer.
- Desktop UI uses Tkinter and requires a GUI session.
- Office exports are lightweight HTML/CSV/basic-PDF workflows, not full DOCX/XLSX/PPTX compatibility.
- Nano is a compact Python source-runtime bundle requiring Python 3.10+, not a native standalone executable.

These are the boundaries of this SDK version, not limits on what future Orbit versions could add.

# 26. Suggested practice projects

Try these in order. Each project combines concepts from earlier chapters.

1. **Greeting app:** ask for a name with `console.read_line` and greet the user.
2. **Number quiz:** use `math.random_int`, `if`, and a loop.
3. **Score tracker:** store player records in a list of objects and save JSON with `data.json_stringify`.
4. **Notes utility:** create, read, and append text files using `orbit.files`.
5. **Drawing tool:** make a canvas, draw shapes, and export a PNG.
6. **Desktop notes window:** use `window`, `ui.entry`, and `ui.button`.
7. **Crystal Hunt game:** modify the Flux example by adding more collectibles, labels, and bounds.
8. **3D rotating-cube study:** experiment with `graphics3d.transform` and save multiple frames.
9. **SQLite high-score table:** create a table, insert scores, and query the results.
10. **Packaged indie game:** build a `.orbit` package, inspect it, and create a Nano player bundle.

## Documentation included with the SDK

For deeper technical detail, read the matching file in `docs/`:

- `ARCHITECTURE.md` — SDK architecture and execution pipeline.
- `INSTALLATION.md` — install and distribution details.
- `LANGUAGE.md` — language grammar and supported syntax.
- `COMPILER.md` — compiler Python API and bytecode format.
- `RUNTIME.md` — VM behavior and runtime boundaries.
- `PACKAGES.md` — `.orbit` packaging and Nano.
- `STANDARD_LIBRARIES.md` — built-in Lux API catalog.
- `FLUX_ENGINE.md` — playable 2D engine details.
- `DESKTOP.md` — desktop UI requirements and APIs.
- `OFFICE.md` — document, spreadsheet, presentation, and PDF APIs.

**Welcome to Orbit. Start with `hello.orb`, experiment with the examples, and build one useful thing at a time.**
