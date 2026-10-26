# Usage

## Installation

1. Download the latest release zip from [Releases](https://github.com/PrPlanIT/DisplayWizard/releases) and unpack the `.exe` or the `.ahk` source file. To run the `.ahk` (plain-text source code) you must install [AutoHotkey](https://www.autohotkey.com).
2. Unpack the [Virtual Display Drivers](https://github.com/VirtualDrivers/Virtual-Display-Driver) (or other IddSample drivers) into the same folder.
3. Install the certificate.
4. Run the `.exe` or the script.
5. How to use it? Read the GUI, do stuff. There are recommendations throughout these docs as well.

Administrator rights are required: reloading the driver needs elevation, and the tool will not run without it.

## Requirements

- **Windows 10/11.**
- **A compatible IddSample driver.** This tool configures implementations of Microsoft's Indirect Display Driver Sample. The one I test with is [the Virtual Display Driver from VirtualDrivers](https://github.com/VirtualDrivers/Virtual-Display-Driver), forked from [ge9's IddSampleDriver](https://github.com/ge9/IddSampleDriver); it will likely work with other forks that share the same IddSampleDriver `.inf` name.
- **nefcon** — driver installation uses pnputil plus [nefcon](https://github.com/nefarius/nefcon) by Nefarius. nefcon ships with the application; there is nothing to install.
- **AutoHotkey v1.x** — only to run the `.ahk` source directly; the `.exe` needs nothing.

## CLI Arguments

Call the `.exe` or script from a staging tool, from Sunshine with admin credentials, or from a `.bat` to load initial settings or install, uninstall or reload the driver. Arguments go directly after the `.exe`; to use them with the `.ahk` script, pipe it through AutoHotkey first.

| Command | |
|---------|-|
| `VirtualDisplayDrv.exe [Driv_Inst / Driv_Unin / Driv_Relo]` | Install, uninstall or reload the driver |
| `VirtualDisplayDrv.exe [Back_Load / Back_Save / Back_Remo] <Name>` | Load, save or remove the backup with that name (in quotes if it contains spaces) |
| `VirtualDisplayDrv.exe Moni_Sets <integer>` | Set the number of monitors |
| `VirtualDisplayDrv.exe GPUs_Sets <string or integer>` | Set the GPU by name, or by its GPU-detection number (in quotes if it contains spaces) |
| `VirtualDisplayDrv.exe [Reso_Adds / Reso_Remo] <WIDTH> <HEIGHT> <Hz>` | Add a resolution to the top of the list (even if it already exists), or remove it |

Adding resolutions dynamically from Sunshine (reloads the driver automagically):

```bat
cmd /C SunshineIntegration.bat %SUNSHINE_CLIENT_WIDTH% %SUNSHINE_CLIENT_HEIGHT% %SUNSHINE_CLIENT_FPS%
```

## Configs and backups
- At runtime it loads `vdd_settings.xml` if present, otherwise `adapter.txt` / `option.txt`. You can replace or omit these at `C:\IddSampleDriver` to work around this very intended behavior.
- Configs are pushed from the script's folder to `C:\IddSampleDriver` under their proper names if they are not already there.
- Config formats convert by injecting one into the other: place any config in a `Backups` folder beside the script, named `option.txt.<backup_title>.backup` (or `vdd_settings.xml…` / `adapter.txt…`).
- You can name a backup whatever you like and place it manually; when you relaunch you can restore from it just as if you had backed it up from the GUI.

## What is Priority?
I made it up 🤷‍♀️ — to force a preferred [width x height + Hz] to the top of the list. Entries with the same [width x height] are grouped by the earliest occurrence of that [w x h], so they stay together when the XML config is written. The only workable way to do this was a bit more robust than I initially wanted, but it resolves an issue I had with Immortals Fenyx Rising that may or may not affect other games or software. [Here, in the issues/solutions segment, I briefly describe it.](http://sh.uni2.cc/28JLJ)

## Known issues
- The GUI must be reloaded to show newly saved backups in the dropdown if another selection is made after saving.
- File writes and priority sorting happen without loading screens — my preferred experience. If you change many things in swift succession, not every change may reach the config before you close. Leave 3–5 seconds after your last change and you're fine even if you go fast (a file write follows each change). CLI usage is unaffected. I resolved this once by hiding the GUI until it was ready, but didn't like it; raise an issue if it troubles you.
- You'll have a generally good experience in the GUI; just be aware of the limitation above. Fixing it without changing the feel means recoding the resolution backend to work in memory rather than in the ListView. I did it this way for fewer lines and less redundant data, and didn't expect these minor performance issues.

> This software is essentially in beta: a quick and dirty tool by design. If you find a bug, let me know — I can't fix what I don't know is broke!

## Planned features
- Validate resolution fields as positive integers so jank can't be inserted into the list.
- Select a default GPU automatically when nothing is selected. Meant to add this already. Whoops!
- Add resolutions by pressing Enter once (Wt x Ht + Hz) is filled out, rather than with a button.
- Uninstall and install the certificate within the driver install/uninstall flows.
- A generic installer flow for users who don't want an advanced tool and just want to get going.
- An interactive installer. These features were considered before the tool was created, and its design allows for them.

## Background

### How did we get here
I use the Virtual Display Driver in my [workstation setup](http://sh.uni2.cc/28JLJ), and at a point I wondered how hard it would be to make a GUI for the driver — and in turn had to figure out how hard it was to finish my thought.

I'm sharing this because I am typically a leecher and not a seeder. I use a lot of open source in my workflow but do not often contribute. I'd be tickled if anyone gets use of this, even if it's only a drop in the bucket.

### Why AutoHotkey?
This was written 100% in [AutoHotkey](https://www.autohotkey.com), which I first touched around 2010. In evaluating how hard this would be, AutoHotkey gave the fastest path from an idea to working code. Every release includes the `.ahk` plain-text source, which you can inspect and change; install AutoHotkey and you can run it directly.

If you do not like AutoHotkey… you are most entitled to that opinion and encouraged to fork and port it to something like Java or Python. I will certainly pull the changes, build a WebUI or do something as a token of gratitude and respect for your opinion. Otherwise, as Deadpool said, MAXIMUM EFFORT!!! 🥴 AutoHotkey it is! 🤣
