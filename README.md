# <img src="docs/assets/app.png" alt="Display Wizard" width="64" height="64" /> Display Wizard

A GUI tool that integrates with software such as [Sunshine](https://github.com/LizardByte/Sunshine) to install, uninstall, reload and configure Indirect Display Driver Sample (IddSample) implementations, like the HDR-capable [Virtual Display Driver from VirtualDrivers](https://github.com/VirtualDrivers/Virtual-Display-Driver), on Windows 10/11 — in place of editing configs by hand or installing drivers through Device Manager.

<!-- sf:project:start -->
[![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/PrPlanIT/DisplayWizard/tree/v0.6.x-autohotkey) [![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/PrPlanIT/DisplayWizard/-/tree/v0.6.x-autohotkey) [![license](https://img.shields.io/badge/license-GPL--3.0-blue)](LICENSE) [![Open Issues](https://img.shields.io/github/issues/PrPlanIT/DisplayWizard)](https://github.com/PrPlanIT/DisplayWizard/issues) [![Contributors](https://img.shields.io/github/contributors/PrPlanIT/DisplayWizard)](https://github.com/PrPlanIT/DisplayWizard/graphs/contributors) [![donate](https://img.shields.io/badge/donate-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/T6T41IT163) [![sponsor](https://img.shields.io/badge/sponsor-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/PrPlanIT)
<!-- sf:project:end -->
<!-- sf:badges:start -->
[![release](https://img.shields.io/github/v/release/PrPlanIT/DisplayWizard?label=release)](https://github.com/PrPlanIT/DisplayWizard/releases) [![downloads](https://img.shields.io/github/downloads/PrPlanIT/DisplayWizard/total)](https://github.com/PrPlanIT/DisplayWizard/releases) [![AutoHotkey](https://img.shields.io/badge/AutoHotkey-v1.x-334455?logo=autohotkey&logoColor=white)](https://www.autohotkey.com) [![platform](https://img.shields.io/badge/platform-Windows%2010%2F11-0078D6?logo=windows&logoColor=white)](docs/README.md#requirements)
<!-- sf:badges:end -->

---

> [!NOTE]
> **v0.6.x, written in AutoHotkey, is the current working version — [download it from Releases](https://github.com/PrPlanIT/DisplayWizard/releases).** It will likely be the last version packaged as AutoHotkey. A Go port is in development on [`main`](https://github.com/PrPlanIT/DisplayWizard/tree/main) but is not close to GA and not ready for use: only its UI is functional. StageFreight, the Dungeon production environment and other work are prioritized separately, so there is no ETA for the Go port.

<img src="docs/screenshots/PPIT-IddSample_Wiz-SS_GUI_Advanced.png" height="400" />

## Quick Start

1. Download the latest release zip from [Releases](https://github.com/PrPlanIT/DisplayWizard/releases) and unpack it.
2. Unpack the [Virtual Display Driver](https://github.com/VirtualDrivers/Virtual-Display-Driver) (or another IddSample driver) into the same folder and install its certificate.
3. Run `PRPlanIT.com-VirtualDisplayDrv_Wiz.exe` as administrator.

To have every [Sunshine](https://github.com/LizardByte/Sunshine) client add and switch to its own resolution, set this as a Sunshine command:

```bat
C:\IddSampleDriver\SunshineIntegration.bat %SUNSHINE_CLIENT_WIDTH% %SUNSHINE_CLIENT_HEIGHT% %SUNSHINE_CLIENT_FPS%
```

See [Sunshine](docs/sunshine/README.md) for the full setup.

### Documentation

| Guide | Covers |
|-------|--------|
| [Usage](docs/README.md) | Installation, requirements, CLI arguments, configs and backups, priority, known issues and planned features |
| [Sunshine](docs/sunshine/README.md) | Have every Sunshine client add and switch to its own resolution |

### Features:

|                                   |                                                                                                   |
| --------------------------------- | ------------------------------------------------------------------------------------------------- |
| **Sunshine integration**          | Adds and switches to each client's requested [width x height x Hz] automatically — set and forget |
| **Driver install / uninstall / reload** | Reload refreshes changes without ending an active Sunshine session                          |
| **Monitors**                      | Set how many virtual displays the driver provides                                                 |
| **Rendering engine (GPU)**        | Detection and selection                                                                           |
| **Resolutions**                   | Add, remove, and group entries of the same [width x height] for the XML config                    |
| **Priority**                      | Force a preferred [width x height + Hz] to the top of the list        |
| **Configs**                       | Read, write and convert both formats (.txt and .xml); push defaults to `C:\IddSampleDriver`        |
| **Backups**                       | Save, load and inject named backups of both config formats from the GUI                           |
| **CLI**                           | Every action above from a command line or `.bat`               |

## Star History
[![Star History Chart](https://api.star-history.com/svg?repos=PrPlanIT/DisplayWizard&type=Date)](https://www.star-history.com/#PrPlanIT/DisplayWizard&Date)

---

## Contributing

- Fork the repository
- Submit Pull Requests / Merge Requests
- [File issues](../../issues/new) with your Windows version, driver version, and the config or command involved

## Credits

* **VirtualDrivers** — the HDR-capable [Virtual Display Driver](https://github.com/VirtualDrivers/Virtual-Display-Driver/tree/0db50a948e4254733ae899b8e13f7517156ab7da) this tool is built around; thank you for bringing HDR to IddSample
* **roshkins** — the [base implementation](https://github.com/roshkins/IddSampleDriver) of Microsoft's Indirect Display Driver Sample
* **ge9** — continued driver work in [their fork](https://github.com/ge9/IddSampleDriver) of roshkins' implementation
* **Nefarius** — [nefcon](https://github.com/nefarius/nefcon), which handles driver installation; thanks for explaining how it works and offering support
* **LizardByte** — [Sunshine](https://github.com/LizardByte/Sunshine), the motivation for the CLI and Sunshine integration; thanks for the advice and kind words
* **timminator** — a [Virtual-Display-Driver fork](https://github.com/timminator/Virtual-Display-Driver) with an interactive installer, whose use of nefcon led to removing DevCon from this project
* **Nonary / Codectory / anaisbetts** — open-source [HDR toggling](https://github.com/Nonary/AutoHDRSwitch)
* **JSRRosenbaum / Jezzick** — Advised removal of wmic ahead of Windows deprecations and also PR for automatic driver downloads.
* Everyone who has contributed to open source projects — thank you

## Disclaimer

> The Software provided hereunder ("Software") is licensed "as-is," without warranties of any kind, express or implied. The Software makes no representations regarding the functionality, performance, compatibility, security, or availability of the Software and no warranty. The developer should not be held responsible even if it blatantly be the cause of your dog running off and joining a circus, or your mother wins 5 tickets to Hawaii but you get stuck on a gaming bender and pass it up. The developer should not be held responsible if use of the Virtual Display Driver causes you to spend longer hours on the PC and you muff up at your workplace. The developer should equally not be accredited with any responsibility for your experience using this software. All thanks is to those in development of the drivers and their forks, and the open source communities! It's never been a better time than now to be a PC user!

## License

Display Wizard is distributed under the [GPL-3.0](LICENSE) license.
