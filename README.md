<div align="center">
<img src="./assets/flint.png" alt="Flint Logo" width="360">
<h1>Flint</h1>
<p><b>The Cargo experience for C/C++—minimalist, Git-native, and CMake-free.</b></p> 
<a href="https://mainak55512.github.io/flint-cherts/"><strong>CLI Docs</strong></a>
<a href="https://mainak55512.github.io/flint-cherts/compositions/"><strong>Chert Index</strong></a>
<a href="https://github.com/mainak55512/flint/issues"><strong>Report Issue</strong></a> 
<br><br>

![Platform](https://img.shields.io/badge/platform-Linux-blue?style=flat-square)

![Status](https://img.shields.io/badge/status-Beta-orange?style=flat-square)

![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
</div>

---

**Flint** brings the modern developer workflow of Rust’s `cargo` or Go modules to C and C++. 

Instead of writing hundreds of lines of complex `CMakeLists.txt` or Makefile glue code, Flint manages dependencies directly from standard Git repositories and drives GCC/Clang compilers automatically through a single JSON manifest.

---

## Demo

<div align="center">
  <br />
  <img src="assets/demo.gif" alt="Flint Terminal Demo" width="85%" />
  <p><sub><i>Flint initializing a workspace, fetching dependencies, and running a build.</i></sub></p>
  <br />
</div>

---

## Features

- **Cargo-Like Simplicity:** Build, sync dependencies, and execute code in a single command.
- **Git-Native Package Management:** Fetch dependencies directly into standard subdirectories using Git URLs.
- **Version & Commit Pinning:** Lock dependencies by release tag, branch, or exact commit hash.
- **Single Manifest (`composition.json`):** One human-readable file replaces entire build script directory structures.
- **Chert Compositions:** Instant compatibility layer for standard C/C++ repositories without a native `composition.json`.
- **Global `VERSION` Macro:** Automatically inject project version headers into C/C++ source code during compilation.
- **Convention Over Configuration:** Clean standard directory layout (`src/`, `include/`, `deps/`).

---

## Quick Start

### Prerequisites
- **OS:** Linux *(Beta)*
- **Compiler:** `gcc` or `clang`
- **Tool:** `git`

### 1. Installation

Install via official script:

```bash
curl -fsSL https://raw.githubusercontent.com/mainak55512/flint/main/install.sh | sh
```

Or build from source:

Bash

```
git clone https://github.com/mainak55512/flint.git
cd flint && ./build.sh
```

### 2. Quick Workflow

```bash
# Initialize a new C project workspace
flint init

# Add a Git dependency
flint add https://github.com/user/example_lib@1.0.0

# Compile and execute immediately
flint run

```

## Project Structure

Flint enforces a clean, zero-config directory model:

```
my_project/
├── src/                # Source files (.c, .cpp)
├── include/            # Local header files (.h, .hpp)
├── deps/               # External dependencies (Managed by Flint)
├── static/             # Static library files (.a)
├── shared/             # Dynamic/Shared library files (.so)
└── composition.json    # Project manifest & build configuration

```

## Manifest Specification (`composition.json`)

```json
{
  "project_name": "example_project",
  "project_language": "c",
  "version": "0.1.0",
  "compiler_path": "/usr/bin/gcc",
  "executable": true,
  "flags": ["-Wall", "-O2"],
  "lib_links": [],
  "include_paths": ["include"],
  "src": ["src"],
  "dependencies": {
    "example_lib": {
      "version": "1.0.0",
      "remote": "[https://github.com/user/example_lib.git](https://github.com/user/example_lib.git)"
    }
  }
}

```

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


## Community & Support

-   **CLI Reference:** [Flint Documentation](https://mainak55512.github.io/flint-cherts/)    
       
-   **Chert Packages:** [Flint Cherts Repository](https://mainak55512.github.io/flint-cherts/compositions/)
             
-   **Bug Reports & Requests:** [GitHub Issues](https://github.com/mainak55512/flint/issues)
    
         
## License

Distributed under the **MIT License**. See `LICENSE` for details.
