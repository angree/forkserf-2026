Forkserf 2026 -- fork by Grzegorz Korycki
=========================================

A 2026 fork of Forkserf (a SerfCity / Settlers 1 clone in C/C++).  The goal of this fork
is to bring the game closer to the original Settlers / Serf City -- in particular its
classic "fast" UI mechanics -- through small, focused changes.

Changes in this fork
--------------------

- **Settlers 1 style double-click to build**: a double left-click on the selected map
  tile builds there, exactly like pressing the "1" panel button.
- **Settlers 2 style road auto-routing**: while building a road, double-click a
  destination flag or a distant tile to auto-route the whole road there (terrain-aware:
  prefers flat ground, avoids water/obstacles), and it snaps to a nearby flag so it is
  easier to hit.
- **Scalable UI ("UI resize")**: a new option (in-game options, page 5) magnifies the
  panel and popups from x1 up to x8 with nearest-neighbour scaling (no blur).  It
  auto-detects the window resolution and never exceeds the magnification that fits
  (shows "MAX" at the ceiling); the default is x3.
- The double-click and special-click triggers (double-click, right+left, middle button)
  are enabled by default.
- UI fixes: the options window stays centred/uncut when the window is resized; the
  message box closes with the tick or ESC; the in-game version number is shown correctly.

AI fixes (0.7.0)
----------------

The advanced (tlongstretch) AI had several behaviours that wrecked its own economy or made it
impossible to play against as an easy opponent.  This fork addresses them:

- **The AI no longer burns its own productive farms / fishers / pig farms.**  An "excess food"
  cull demolished food producers whenever finished food was briefly high, which broke the
  wheat -> mill -> baker chain and caused an endless build/burn loop (the farm burned even when
  fully road-connected, and was often never rebuilt).  Surplus production now simply idles.
- **No more burning "excess" lumberjacks / foresters**, nor the unproductive 3rd lumberjack "to
  relocate it" -- the same wasteful build/burn churn for no real gain.
- **The AI no longer burns a building (or a freshly placed mine) just because its flag is
  momentarily not road-connected.**  Transient road churn used to make it torch productive
  buildings; now it keeps them and retries connecting each loop.  Only depleted mines and
  out-of-stone stonecutters are still demolished (the only cases that make sense).
- **The Intelligence slider works again as a real difficulty setting.**  The advanced AI ignored
  the original Intelligence value (the slider was disabled and the AI always played at full
  strength), so there was no way to set up an easier opponent.  Lowering Intelligence now
  rate-limits how often that AI places new buildings (per-player, measured in game ticks), so a
  low-IQ opponent develops slower -- without ever stalling its economy.
- **The "ALL" statistic is a real composite again.**  The combined score used a hugely inflated
  military term, so "ALL" was effectively a 1:1 copy of the military score and hid economic/land
  progress.  It is now a balanced mix of buildings + land + military (display only; the winner
  logic is unchanged).

Other fixes / tools
-------------------

- **Map dragging fixed**: dragging the view with the right (or left) button no longer fires a
  stray click on release (which used to close the popup/minimap you were panning over).
- **Sprite export tool**: run `Forkserf -E DIR` to dump every game sprite to PNG (composited onto
  a white background) into DIR, then exit -- handy for modding or editing the graphics.

Based on Forkserf's `stable` branch (upstream release v0.6.3).  Forkserf is a continuation
of Freeserf (created by jonls and wdigger).  Upstream: https://github.com/forkserf/forkserf


Forkserf (upstream README follows)
==================================

Game Information Website
========================

- https://forkserf.github.io/
- Discord channel 'Forkserf'

Current Release
===============

version 0.7.0 (fork by Grzegorz Korycki, 2026) -- based on upstream Forkserf release 0.6.3


Play
------
Copy the data file(s) from the original game to the same directory as freeserf. Alternatively you can put the data file in `~/.local/share/freeserf`. You may use data file(s) from DOS or Amiga game version.  If available, it is recommended to include BOTH Amiga and DOS files as Forkserf will use the best of both asset sets.

* DOS data file is called `SPAE.PA`, `SPAD.PA`, `SPAF.PA` or `SPAU.PA`, depending on the language of the game.
* Amiga files `gfxheader`, `gfxfast`, `gfxchip`, `gfxpics`, `sounds`, `music`.

Keyboard gameplay controls:

* `1`, `2`, `3`, `4`, `5`: Activate one of the five buttons in the panel.
* `b`: Toggle overlay showing possibilities for constructions.  Can also be brought up by special-clicking on Build icon in panel bar
* `+`/`-`: Increase/decrease game speed.  Default is 2, can go up to 40
* `0`: Reset default game speed
* `p`: Pause game.  Also pauses AI player logic thread at the start of their next loop
* `j`: Switch player, you can control even AI players while they play, though it might cause instability if you go too crazy with it
* `y`: AI info overlay (only shows for AI players)
* `d`: Debug overlay
* `g`: Grid/Serf-State debug overlay
* `w`: Enable/disable Four Seasons graphics (it is no longer tied to AdvancedFarming though it is recommended to use them together)
* `f`: Toggle FogOfWar
* `t`: Play next music track, switches between DOS and Amiga music if both available and last track reached
* `s`: Toggle sounds playback
* `m`: Toggle music playback
* `h`: Hidden resource overlay (THIS IS CHEATING!)
* `i`: Next mine popup (cycle through player's Mines)
* `CTRL`+`f`: Switch fullscreen mode on/off.  (should add ALT-ENTER at some point also)
* `CTRL`+`z`: Quicksave game in current directory.
* `[`/`]`: Zoom -/+
* `MouseWheel`: Zoom -/+     
* `CTRL`+`n` or `F10`: Raise game-init popup, can start a new game
* ~~`TAB`/SHIFT-`TAB`: Open next notification message; or return from last message.~~ removed for now because of alt-tab issues
* `ESCAPE`: Close current popup (unless it is moved/pinned).  Can also use right-click to do this

Mouse:
* most operations left-click
* double left-click, OR "special-click" (both left and right at same time), OR center-button/mousewheel-click, OR right-click to trigger original game "special-click" functions
* click and drag the viewport to scroll
* click and drag popup windows to move them around the game window.  If done, multiple windows can be opened at the same time and will auto-refresh
* right click anywhere to close popup window (unless it was moved, then it will stay until closed with its close button)


Audio
-----

To play back the sound track that is included in the original data files,
SDL2_mixer has to be enabled at compile-time and a set of sound patches
for SDL2_mixer has to be available at runtime. See the SDL2_mixer
documentation for more information.


Save games
----------
To load a save game file:

`$ Forkserf -l FILE`

Freeserf will (try to) load save games from the original game, as well as saves from freeserf itself.
The game is paused after loading so press `p` to start the game.

Run `Forkserf -h` for more info on command line options.


Bugs
----
Please report bugs at <https://github.com/forkserf/forkserf/issues>.

Development
-----------
The main source repository for this project is at <https://github.com/forkserf/forkserf>

back in business, for 2022-2023 winter

