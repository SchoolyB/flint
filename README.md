<div align="center">
  <img src="./assets/flint.png" alt="Flint Logo" width="360">
  <h1>Flint</h1>
  <p><b>A minimalist, Git-native build system & package manager for C/C++</b></p>

  <a href="https://mainak55512.github.io/flint-cherts/"><strong>CLI Documentation</strong></a> •
  <a href="https://mainak55512.github.io/flint-cherts/compositions/"><strong>Chert Compositions</strong></a>
</div>

---

> ⚠️ **Disclaimer:** Flint is currently in **BETA** and natively available for **Linux**. Use with caution in production environments.

Flint simplifies C/C++ development by eliminating complex build scripts. It fetches dependencies directly via Git, manages build configurations through a single `composition.json` manifest, and orchestrates GCC/Clang compilers automatically.

---

## Demo

<div align="center">
  <br />
  <img src="assets/demo.gif" alt="Flint Terminal Demo" width="85%" />
  <p><sub><i>Flint initializing a workspace, adding dependencies, and running a build.</i></sub></p>
  <br />
</div>

## Features

- **Git-Native Package Management:** Fetch dependencies directly into your project using standard Git repositories.
- **Version & Commit Pinning:** Lock dependencies by tag or commit hash for reproducible builds.
- **Manifest-First (`composition.json`):** Single JSON file manages compiler options, include paths, and link flags—no complex Makefile or CMake scripts required.
- **Chert Compositions:** Compatibility layer that allows third-party libraries without a `composition.json` to work out of the box.
- **Global `VERSION` Macro:** Automatically injects your project's version directly into your source code during compilation.
- **Zero-Config Execution:** Build and instantly execute binaries using `flint run`.

---

## Quick Start

### Prerequisites

- **OS:** Linux
- **Compiler:** GCC or Clang
- **Tools:** Git

### Installation

Install Flint via the official one-liner script:

```bash
curl -fsSL -H "Accept: application/vnd.github.v3.raw" https://api.github.com/repos/mainak55512/flint/contents/build.sh | bash

```

---

## Project Structure

Flint uses a clean, conventional directory structure:

```text
my_project/
├── src/                # Source files (.c, .cpp)
├── include/            # Local header files (.h, .hpp)
├── deps/               # External dependencies (Managed by Flint)
├── static/             # Static library files (.a)
├── shared/             # Dynamic/Shared library files (.so)
└── composition.json    # Project manifest & build configuration

```

---

## Usage & Workflow

### 1. Initialize a Project

Create a new Flint workspace:

```bash
flint init

```

### 2. Add Dependencies

Add a library directly from a Git URL:

```bash
flint add <git_remote_url>@<version>

```

Alternatively, add third-party [Chert Compositions](https://mainak55512.github.io/flint-cherts/compositions/?utm_source=gemini) directly into the `dependencies` block of your `composition.json` and sync:

```bash
flint sync

```

### 3. Remove a Dependency

Un-track and remove a managed dependency:

```bash
flint remove <repo_name>

```

### 4. Build and Run

Compile your project:

```bash
flint build

```

Or compile and execute immediately in one step:

```bash
flint run

```

---

## `composition.json` Specification

The project manifest controls compilation flags, paths, and dependency tracking:

```json
{
  "project_name": "example_project",
  "project_language": "c",
  "version": "0.1.0",
  "compiler_path": "/usr/bin/gcc",
  "executable": true,
  "flags": [],
  "lib_links": [],
  "include_paths": ["include"],
  "src": ["src"],
  "dependencies": {
    "example_lib": {
      "version": "1.0.0",
      "remote": "[https://github.com/user/example_lib](https://github.com/user/example_lib)"
    }
  }
}

```

---

## How Flint Compares

| Feature / Attribute | Flint | CMake + vcpkg | cmkr + FetchContent | Meson + WrapDB | XMake + Xrepo | Bazel |
|---|---|---|---|---|---|---|
| Tool Architecture | Integrated Build System & PM | Meta-Build Generator + Standalone PM | Meta-Wrapper Generator (outputs CMakeLists.txt) | Meta-Build Generator + Package Resolver | Integrated Build System & PM | Multi-language Distributed Build System |
| Runtime Dependencies | System git, gcc/clang | C++ runtime, git, build backend (ninja/make) | cmkr, CMake, git, build backend | Python 3, ninja, git | Embedded Lua Engine, system compiler | Java Runtime Environment (JVM) |
| Config Format | composition.json (JSON) | CMakeLists.txt + vcpkg.json (Custom DSL + JSON) | cmake.toml (TOML) | meson.build (Declarative DSL) | xmake.lua (Lua scripts) | BUILD / WORKSPACE (Starlark) |
| Package Management Method | Git-native clones to deps/ via CLI (flint add) | Manifest/Port-tree repos & binary caching | CMake FetchContent (configure-time download) | Wrap files (.wrap) & WrapDB registry | Native Xrepo index + fallbacks (vcpkg/Conan) | Remote repositories & Bazel modules (Bzlmod) |
| Non-Native Library Handling | Chert Compositions (custom specs for non-Flint repos) | vcpkg Port Overlay recipes | Requires manual CMake target wrapping | Meson Wrap subproject patches | Xrepo package build scripts | Custom Starlark rule definitions |
| Incremental Build Engine | Native state/modification tracking | Delegated to backend (Ninja/Make) | Delegated to CMake backend | Delegated to Ninja | Native Lua task engine | Content-hash DAG dependency graph & cache |
| Directory Model | Convention-over-configuration (src/, include/, deps/) | Fully explicit & customisable | Explicit & customisable | Explicit & customisable | Flexible / semi-conventional | Explicit package rule targets |
| Platform Support | Linux only (Current) | Cross-Platform (Linux, macOS, Windows) | Cross-Platform (Linux, macOS, Windows) | Cross-Platform (Linux, macOS, Windows) | Cross-Platform (Linux, macOS, Windows) | Cross-Platform (Linux, macOS, Windows) |
| Cross-Compilation | Flags passed to system compiler | Toolchain files (-DCMAKE_TOOLCHAIN_FILE) | Toolchain files via CMake | Cross-definition files (--cross-file) | Built-in CLI flags (--sdk, --plat) | Hermetic platform toolchains |
| Ease of Use | 4.8 / 5 | 2.5 / 5 | 4.0 / 5 | 4.5 / 5 | 4.5 / 5 | 1.5 / 5 |

---

## Documentation & Community

* **CLI Reference:** [Flint CLI Docs](https://mainak55512.github.io/flint-cherts/)
* **Chert Index:** [Flint Cherts Repository](https://mainak55512.github.io/flint-cherts/compositions/)
* **Issues & Feedback:** [GitHub Issues](https://github.com/mainak55512/flint/issues)

---

## License

Distributed under the MIT License. See `LICENSE` for details.

