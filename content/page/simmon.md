---
title: SimMon
date: Friday, February 19th, 2021
thumbnail: /images/uploads/sm_poster.webp
category: Tools
createdAt: 2020-02-18T17:49:25.117Z
---
**Start any follow up application when the start of your Sim application is detected.**

[Download latest Release](https://github.com/tappi287/simmon_gui/releases/latest)

<p align="center">
    <img src="https://github.com/tappi287/simmon_gui/raw/master/ui/screenshot_092.png" alt="Screenshot" width="480">
</p>

#### SimMon Gui
Create profiles of what applications to start or cmd's to execute once the start of your Sim was detected.
Followup chain detection of other running processes for different tasks:
- "myawesomesim.exe" started
    - Condition Steam-VR running -> start CrewChief and SimHub to drive
    - Condition OBS running -> start fancy streaming app

Auto detection of some common Sim Games by their registry keys.

#### SimMon Watcher
Part of this application is the Background Watcher. Low profile background app/Windows Service that watches for process creation/termination. Fulfil profile tasks
created by the GUI.

Controllable from within the GUI. No need to keep the GUI running. Just close it.
The watcher will relentlessly run in the background.
