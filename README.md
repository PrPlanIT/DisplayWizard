# <img src="docs/assets/app.png" alt="Display Wizard" width="64" height="64" /> Display Wizard

Manages a virtual display driver on Windows: installs, removes and reloads the driver and configures virtual monitors. A Go backend (`dwiz`, a CLI and a loopback HTTP API) with a React UI in an Electron shell.

<!-- sf:project:start -->
[![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/PrPlanIT/DisplayWizard) [![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/PrPlanIT/DisplayWizard) [![license](https://raw.githubusercontent.com/PrPlanIT/DisplayWizard/main/.stagefreight/scribe/license.svg)](https://github.com/PrPlanIT/DisplayWizard/blob/main/LICENSE) [![Open Issues](https://img.shields.io/github/issues/PrPlanIT/DisplayWizard)](https://github.com/PrPlanIT/DisplayWizard/issues) [![Open PRs](https://img.shields.io/github/issues-pr/PrPlanIT/DisplayWizard)](https://github.com/PrPlanIT/DisplayWizard/pulls) [![Contributors](https://img.shields.io/github/contributors/PrPlanIT/DisplayWizard)](https://github.com/PrPlanIT/DisplayWizard/graphs/contributors) [![donate](https://img.shields.io/badge/donate-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/T6T41IT163) [![sponsor](https://img.shields.io/badge/sponsor-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/PrPlanIT)
<!-- sf:project:end -->
<!-- sf:badges:start -->
[![release](https://raw.githubusercontent.com/PrPlanIT/DisplayWizard/main/.stagefreight/scribe/release.svg)](https://github.com/PrPlanIT/DisplayWizard/releases) [![build](https://raw.githubusercontent.com/PrPlanIT/DisplayWizard/main/.stagefreight/scribe/build.svg)](https://gitlab.prplanit.com/PrPlanIT/DisplayWizard/-/pipelines) [![Go Version](https://img.shields.io/github/go-mod/go-version/PrPlanIT/DisplayWizard?logo=go)](https://github.com/PrPlanIT/DisplayWizard) [![Go Reference](https://pkg.go.dev/badge/github.com/PrPlanIT/DisplayWizard.svg)](https://pkg.go.dev/github.com/PrPlanIT/DisplayWizard) [![Last Commit](https://img.shields.io/github/last-commit/PrPlanIT/DisplayWizard)](https://github.com/PrPlanIT/DisplayWizard/commits) [![StageFreight](https://img.shields.io/badge/StageFreight-0.14.0--dev+3f49b77-310937?logo=readthedocs&logoColor=white)](https://stagefreight.prplanit.com)
<!-- sf:badges:end -->

---

> [!WARNING]
> **This is the Go port, in development and not close to GA: only its UI is functional.** The working version is v0.6.x, written in AutoHotkey, on the [`v0.6.x-autohotkey`](https://gitlab.prplanit.com/PrPlanIT/DisplayWizard/-/tree/v0.6.x-autohotkey) branch — [download it from Releases](https://github.com/PrPlanIT/DisplayWizard/releases). There is no ETA for the Go port.

<img src="docs/screenshots/DisplayWizard-GUI.png" height="400" alt="Display Wizard" />

### Features:

|                                   |                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------- |
| **Driver lifecycle**              | Install, remove and reload the virtual display driver                               |
| **Virtual monitors**              | Configure the monitors the driver provides                                          |
| **CLI and API**                   | `dwiz` runs every operation from a command line, or serves them on `127.0.0.1:5757` |
| **Per-operation elevation**       | Runs as the invoking user; only driver install, remove and reload ask for UAC      |

### Documentation

| Guide | Covers |
|-------|--------|
| [Design](docs/design.md) | Architecture, direction and phases, security invariants |

---

## Contributing

- Fork the repository
- Submit Pull Requests / Merge Requests
- [File issues](https://github.com/PrPlanIT/DisplayWizard/issues/new) with your Windows version, driver version, and the command involved

## Credits

* **VirtualDrivers** — the HDR-capable [Virtual Display Driver](https://github.com/VirtualDrivers/Virtual-Display-Driver) this tool is built around
* **Nefarius** — [nefcon](https://github.com/nefarius/nefcon), which handles driver installation
* **LizardByte** — [Sunshine](https://github.com/LizardByte/Sunshine), the motivation for the CLI and Sunshine integration

## License

Display Wizard is distributed under the [AGPL-3.0](LICENSE) license.
