# JSRF Unleashed — Nintendo Switch 2 (`libounce` / `kaiHEN`)

A Switch 2-compatible build of **JetSetRadioFuture-Unleashed-NX**, patched to run under `libounce` and `kaiHEN` / `hbmenu` without triggering Switch 1 Applet Mode checks or privileged kernel memory syscalls.

> **Disclaimer:** No copyrighted game assets are included in this release. You must provide your own legally dumped **USA (NTSC-U)** retail copy of *Jet Set Radio Future*.

---

## Requirements

* **Nintendo Switch 2** running `kaiHEN` / `hbmenu`
* **Retail USA (NTSC-U)** *Jet Set Radio Future* disc dump *(standalone US retail release; PAL and Sega GT 2002 combo discs have different XBE layouts)*
* An XISO extraction tool (such as **extract-xiso**) to unpack the disc files

---

## Installation

### 1. Extract Your Game Files
Extract the contents of your **USA (NTSC-U)** *Jet Set Radio Future* ISO (`default.xbe`, `media/`, `audio/`, etc.) into a folder named **`game`**.

### 2. Copy Files to Your microSD Card
Download **`jsrf-unleashed.nro`** from the [Releases](../../releases) page and place it alongside your extracted **`game`** folder on your microSD card:

```text
sdmc:/
└── switch/
    └── jsrf-unleashed/
        ├── jsrf-unleashed.nro
        ├── options.txt
        └── game/
            ├── default.xbe
            └── (remaining extracted disc files & folders)
```

### 3. Recommended `options.txt`
Create or update `sdmc:/switch/jsrf-unleashed/options.txt` with the following settings for best performance on Switch 2:

```ini
widescreen=1
scale=1
aniso=1
fps=60
RECOMP_VSYNC=0
shader_cache=1
fast_gpu=1
show_fps=1
log=0
log_direct=0
```

> **Important:** Keep `log=0` and `log_direct=0` unless debugging. Synchronous SD card logging severely degrades framerate.

---

## Launching & Controls

1. Insert your microSD card into your Switch 2 and open **`hbmenu`**.
2. Select **JSRF Unleashed NX** (`jsrf-unleashed.nro`).
3. **First-Boot Caching:** On your very first launch, the port compiles its OpenGL ES / Vulkan shader cache (`save/shaders/`) and populates the Xbox utility cache (`save/Cache/`). Give it ~10–15 seconds to finish; subsequent boots will load much faster.
4. **In-Game Settings Menu:** Tap or hold **`Minus`** (or press **`L + R`** together) at any time in-game to open the built-in settings overlay to adjust resolution **`scale`**, **`widescreen`**, and VSync live.

---

## What Was Changed for Switch 2

* **Bypassed Switch 1 `hbloader` Syscall Hints (`src/platform/switch_mem.c`)**: Switch 2's homebrew loader does not expose `libnx` syscall hints (`0x73`–`0x78`) or an `hbloader` process handle, which previously caused the port to exit immediately with a false Applet Mode error.
* **Flat-Span Userland Memory Allocator**: Replaced privileged `svcMapProcessCodeMemory` / `svcSetProcessMemoryPermission` calls with a `libounce` flat-span heap allocator so `xbox_MemoryLayoutInit` initializes cleanly without kernel SVC crashes.
* **OpenGL ES Renderer (`-DXBOXRECOMP_DEKO3D=OFF`)**: Built using the SDL2 + OpenGL ES rendering path instead of Switch 1 `deko3d`.
