<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="logo-dark.svg">
    <img src="logo-light.svg" alt="Scouren" width="560">
  </picture>
</p>

<p align="center">
  Analog and mixed-signal IC design tools, from schematic to sign-off.<br>
  <a href="https://scouren.com"><b>scouren.com</b></a> ·
  <a href="https://github.com/Scouren/Schemify-Releases/releases/latest">Download Schemify</a> ·
  <a href="#open-source">Open source</a>
</p>

## Schemify

<p align="center">
  <img src="screenshots/steps/03-sim.png" alt="Schemify after a simulation: the RC bench, its docs page, and the Results pane with the run's measurements and log" width="860">
</p>

**Schemify is our schematic editor for analog and mixed-signal IC design.** It's a fast, lightweight replacement for xschem, written in Rust. It draws schematics, writes netlists for ESpice, ngspice, Xyce, LTspice, HSPICE, CDL, Spectre and Verilog, runs the simulation and shows the waveforms. It imports xschem libraries and PDKs, so you can switch without redrawing anything.

<p align="center">
  <img src="screenshots/docs-pane.png" alt="Schemify showing a five-transistor OTA beside its docs page, where every transistor width is computed from a gm/Id target" width="860">
</p>

- **Docs next to the circuit.** Each cell carries a docs page with Markdown, LaTeX and live gm/Id sizing math that updates the schematic when a value changes.
- **Plain-text designs.** One line per object, so a change is a one-line diff, and `schemify merge-driver` merges branches object by object.
- **Safe to open.** Opening a file never runs anything. Simulator commands live in your own config and in plugins.
- **A small core with plugins, like Neovim.** Plugins are sandboxed wasm with the permissions you grant. Even the xschem-style menus are a plugin you can replace.
- **The same netlist everywhere.** Byte-identical output on Linux, macOS, Windows and WASI, with xschem's default keys.

### From schematic to simulation, step by step

| | |
|---|---|
| ![The Explorer beside a schematic](screenshots/steps/05-explorer.png) | ![Drawing a wire](screenshots/steps/06-wiring.png) |
| 1. Open a project: cells, outline and nets | 2. Draw and wire the schematic |
| ![The netlist written through SPRack](screenshots/steps/11-netlist.png) | ![Simulation results and run log](screenshots/steps/03-sim.png) |
| 3. Netlist it (any dialect, through SPRack) | 4. Simulate: measurements, specs and the run log |
| ![The waveform viewer](screenshots/steps/04-waves.png) | ![A docs page variable edit recomputing the page](screenshots/steps/12-docs-edit.png) |
| 5. Read the waveforms | 6. Size it on the docs page: values recompute live |
| ![The generated layout open in KLayout](screenshots/steps/10-klayout.png) | |
| 7. Generate the layout and open it in KLayout | |

### The agent (in progress)

| | | |
|---|---|---|
| ![The agent asks before installing anything](screenshots/steps/07-agent-ask.png) | ![The agent stages its edit for review](screenshots/steps/08-agent-staged.png) | ![Accepted: the schematic redrawn with the new width](screenshots/steps/09-agent-accepted.png) |
| It asks before anything it can't undo | Edits are staged for your review | Accept, and the schematic is redrawn |

### Install

```sh
brew install scouren/schemify/schemify          # macOS and Linux
nix run github:Scouren/Schemify-Releases        # Nix and NixOS
```

There are also `.deb`, `.rpm`, Arch and Windows builds on the [releases page](https://github.com/Scouren/Schemify-Releases/releases/latest) and at [scouren.com](https://scouren.com). To build plugins and themes, start with the [Schemify plugin SDK](https://github.com/Scouren/Schemify-Plugins).

## Open source

Schemify is built on open tools that also work on their own beside the ones you already use: SPICE simulation in **ESpice**, Verilog-A device models compiled by **VerA**, and DRC, LVS, ERC and PEX in **GPurify**.

```mermaid
flowchart LR
  T["AMTemplate<br/>project template"] --> S["Schemify<br/>schematic"]
  S -->|netlist| E["ESpice<br/>simulation"]
  V["VerA<br/>Verilog-A models"] --> E
  S -->|netlist for LVS| G["GPurify<br/>DRC · LVS · PEX"]
```

```sh
nix run github:Scouren/ESpice -- deck.sp        # simulate a SPICE deck
```

To start a chip or block, create a repository from [AMTemplate](https://github.com/Scouren/AMTemplate) (**Use this template**), then run `make -C .flows CreateProject PROJECT_NAME=my_chip PROJECT_TYPE=mixed`.

These tools are young and move quickly. The chart shows how much of each project's tracked work is done and how active it has been lately. A daily job updates it.

<!-- status:start -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="status-dark.svg">
  <img src="status-light.svg" alt="Project status: issues closed and open, and commits per week, for each Scouren project. The table below lists the same projects.">
</picture>

| Group | Project | What it does | Status |
|---|---|---|---|
| Schemify ecosystem | [Schemify-Plugins](https://github.com/Scouren/Schemify-Plugins) | Schemify SDK: file format, plugin ABI, SDKs and sandbox host for building Schemify plugins and themes | [![CI](https://github.com/Scouren/Schemify-Plugins/actions/workflows/ci.yml/badge.svg)](https://github.com/Scouren/Schemify-Plugins/actions/workflows/ci.yml) ![last commit](https://img.shields.io/github/last-commit/Scouren/Schemify-Plugins?label=last%20commit) |
| Schemify ecosystem | [GmIdW](https://github.com/Scouren/GmIdW) | Gm&IdW: gm/Id design visualizer (Schemify plugin) | [![CI](https://github.com/Scouren/GmIdW/actions/workflows/ci.yml/badge.svg)](https://github.com/Scouren/GmIdW/actions/workflows/ci.yml) ![last commit](https://img.shields.io/github/last-commit/Scouren/GmIdW?label=last%20commit) |
| Schemify ecosystem | [Interoperate](https://github.com/Scouren/Interoperate) | Schemify's Import/Export working with other Schematic Editors so you can use it interchangeably | [![CI](https://github.com/Scouren/Interoperate/actions/workflows/ci.yml/badge.svg)](https://github.com/Scouren/Interoperate/actions/workflows/ci.yml) ![last commit](https://img.shields.io/github/last-commit/Scouren/Interoperate?label=last%20commit) |
| Schemify ecosystem | [Schemify-Themes](https://github.com/Scouren/Schemify-Themes) | spicetify's theme catalogue for Schemify: 16 themes, 115 colour schemes, View → Themes | [![CI](https://github.com/Scouren/Schemify-Themes/actions/workflows/gate.yaml/badge.svg)](https://github.com/Scouren/Schemify-Themes/actions/workflows/gate.yaml) ![last commit](https://img.shields.io/github/last-commit/Scouren/Schemify-Themes?label=last%20commit) |
| Simulation | [ESpice](https://github.com/Scouren/ESpice) | Fast Spice Simulations, with Verilog-AMS support using VerA | [![CI](https://github.com/Scouren/ESpice/actions/workflows/test.yml/badge.svg)](https://github.com/Scouren/ESpice/actions/workflows/test.yml) ![last commit](https://img.shields.io/github/last-commit/Scouren/ESpice?label=last%20commit) |
| Simulation | [VerA](https://github.com/Scouren/VerA) | A Verilog-AMS Compiler with SIMD and GPU support as first-class residents | [![CI](https://github.com/Scouren/VerA/actions/workflows/test.yaml/badge.svg)](https://github.com/Scouren/VerA/actions/workflows/test.yaml) [![release](https://img.shields.io/github/v/release/Scouren/VerA?label=release)](https://github.com/Scouren/VerA/releases/latest) ![last commit](https://img.shields.io/github/last-commit/Scouren/VerA?label=last%20commit) |
| Verification | [GPurify](https://github.com/Scouren/GPurify) | A SIMD-First DRC, ERC, LVS, PEX engine, built for better checks than propietary work | [![CI](https://github.com/Scouren/GPurify/actions/workflows/test.yml/badge.svg)](https://github.com/Scouren/GPurify/actions/workflows/test.yml) ![last commit](https://img.shields.io/github/last-commit/Scouren/GPurify?label=last%20commit) |
| Project setup & standards | [AMTemplate](https://github.com/Scouren/AMTemplate) | Template for analog and mixed-signal ASIC design on open-source tools | [![CI](https://github.com/Scouren/AMTemplate/actions/workflows/ci.yaml/badge.svg)](https://github.com/Scouren/AMTemplate/actions/workflows/ci.yaml) ![last commit](https://img.shields.io/github/last-commit/Scouren/AMTemplate?label=last%20commit) |
| Project setup & standards | [Conformance-Suite](https://github.com/Scouren/Conformance-Suite) | A conformance suite for Verilog-AMS (Containing Verilog-A & IEEE 1364 specifics) | [![CI](https://github.com/Scouren/Conformance-Suite/actions/workflows/conformance.yml/badge.svg)](https://github.com/Scouren/Conformance-Suite/actions/workflows/conformance.yml) ![last commit](https://img.shields.io/github/last-commit/Scouren/Conformance-Suite?label=last%20commit) |
| Other | [SPRack](https://github.com/Scouren/SPRack) | A Spice Interface in Python that has universal syntax, emits any SPICE backend of the user's choice | [![release](https://img.shields.io/github/v/release/Scouren/SPRack?label=release)](https://github.com/Scouren/SPRack/releases/latest) ![last commit](https://img.shields.io/github/last-commit/Scouren/SPRack?label=last%20commit) |
| Other | [Schemify-Git](https://github.com/Scouren/Schemify-Git) | Git version control for Schemify: the Source control pane, status badges, diff and annotate (a Schemify plugin) | ![last commit](https://img.shields.io/github/last-commit/Scouren/Schemify-Git?label=last%20commit) |
| Other | [schemify-index](https://github.com/Scouren/schemify-index) | Schemify plugin and theme marketplace: listings by pull request, reviewed and signed by Scouren | ![last commit](https://img.shields.io/github/last-commit/Scouren/schemify-index?label=last%20commit) |
<!-- status:end -->

## Support the open-source tools

The open-source projects are partly community funded. To sponsor the work, or to sponsor by invoice as a company, email [sponsors@scouren.com](mailto:sponsors@scouren.com).
