VIctory (cool_fox) on an RX 9070 XT

A compatibility layer fork based on VIctory (with lineage from codec-d/cool_fox, fxceless/victory-fork, Belobug/VIctory-Belobug-AMD, and avatarmanz/VIctory-7900XTX), tuned and tested locally around running an Xbox Series title on Windows 11 with an AMD Radeon RX 9070 XT (RDNA 4) and Ryzen 5 5600G.

This repository contains no game files, code, or assets. You must supply your own local dump of the title. This project is not affiliated with or endorsed by the game's developer or publisher.

Quick Start (cool_fox)

Open PowerShell in D:\Games\GET GTA 6 RUNNING ON WINDOWS 11\cool_fox, point VICTORY_GAME_DIR at your local game directory, and launch via Play-VIctory.cmd (or choose the cool_fox entry in Run-GTA6-VIctory.cmd):

Set-Location 'D:\Games\GET GTA 6 RUNNING ON WINDOWS 11\cool_fox'
$env:VICTORY_GAME_DIR = 'D:\Games\GET GTA 6 RUNNING ON WINDOWS 11\GTA6'
.\Play-VIctory.cmd pareto-residency


Play-VIctory.cmd configures the Series S title profile, 1080p display mode, and cache path (%LOCALAPPDATA%\VIctory\cache-pareto unless VICTORY_CACHE_DIR is set), and applies default VICTORY_* environment variables only when not already defined in your shell.

Building cool_fox

Requirements:

Windows 10/11 x64

CPU with AVX2 support

Direct3D 12 GPU with Shader Model 6.6 and mesh shader support (tested on Radeon RX 9070 XT)

Rust toolchain and MSVC build tools with a Windows SDK

Build a native-optimized release binary (target\release\victory.exe):

$env:RUSTFLAGS = '-C target-cpu=native'
cargo build --release --locked -p victory --config 'profile.release.lto="thin"'


Play-VIctory.cmd automatically uses the built victory.exe target when present.

Performance & Memory Tuning on 32 GB RAM

On a 32 GB DDR4 system (where ~27.9 GB is usable by Windows after hardware reservation), system RAM and per-frame draw-call translation are the primary limits rather than a frame-rate cap alone.

Useful Environment VariablesVariableTypical Value UsedPurposeVICTORY_CARD_BUFFERS_MB1024 (or 768 / 512 under tight RAM)Caps card buffer memory allocation to reduce RAM pressureVICTORY_TEXTURE_BUDGET_MB6144 or 8192Caps copied/budgeted title texturesVICTORY_TEXTURE_DROP_MIPS2 or 3Drops top mip levels to lower staging memory and GPU bandwidthVICTORY_COPY_PLAIN1Uses plain copy path to reduce copy overheadVICTORY_PIPELINE_WAIT_MS24Bounds per-frame wait time for background pipeline creationVICTORY_SHADER_LOOP_LIMIT2048 or 4096Caps shader loop iteration limit to curb long frame spikesVICTORY_LOGerrorReduces console logging overhead during runsPractical Tips for Lower Frame Times & Fewer StallsClose RAM-heavy background apps before launching: Browsers, web-UI apps, overlays, and launchers compete directly with victory.exe when memory climbs above 90–95%.Run one victory.exe instance at a time: Always close any running victory.exe before editing Play-VIctory.cmd or starting a new session.Avoid stacking conflicting flags blindly: Test changes one step at a time and check both title-bar ms/frame / draws/frame and Task Manager memory headroom.

Current Status & Known Issues

Observed behavior on Windows 11 (Ryzen 5 5600G, 32 GB DDR4, Radeon RX 9070 XT, 1920x1080, Series S profile):

Menus / front-end: ~30 FPS with low draw counts (tens of draws per frame) and warm pipeline cache.

Active scenes / corridors: Heavy draw counts (thousands of draws per frame) result in single-digit to low-teens FPS and multi-hundred-millisecond spikes when memory or shader/pipeline translation is saturated.

Memory ceiling: Heavy scenes can push total system RAM near 95% (~26.5+ GB in use on a 27.9 GB usable pool), increasing pagefile activity.

Visual & stability caveats: Lighting, reflections/bright windows, streaming textures, animation/shader edge cases, and occasional D3D12 / AMD driver (amdxc64.dll) faults remain active areas of investigation in early builds.

Credits & Lineage

Built on VIctory and shaped by work and ideas across the community, including codec-d/cool_fox, fxceless/victory-fork, DexNom/VIctory, Belobug/VIctory-Belobug-AMD, avatarmanz/VIctory-7900XTX, myokex/victory-amd, FQY55kai/ZJOU-VICTORY, and contributors (Geckomacabre, cxy0072019-sketch, and upstream authors).
