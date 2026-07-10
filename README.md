# XStoreUnlocker

DLC unlocker for Microsoft Store and Xbox PC games. Works on games that use the XStore API for validating DLC ownership.

## Install

You need three files in your game folder:

| File | What it is |
|------|------------|
| `XGameRuntime.dll` | Our proxy DLL (from the release zip) |
| `XGameRuntime_o.dll` | The real Xbox runtime (you provide this) |
| `xstore_unlocker.ini` | Config file (from the release zip, or auto-created on first launch) |

Steps:

1. Copy `XGameRuntime.dll` from `C:\Windows\System32` into your game folder
2. Rename it to `XGameRuntime_o.dll`
3. Drop our `XGameRuntime.dll` and `xstore_unlocker.ini` from the release zip into the same folder
4. Launch the game

Your game folder should look like this:

```
GameFolder/
  game.exe
  XGameRuntime.dll          <- ours (proxy)
  XGameRuntime_o.dll        <- the original from System32
  xstore_unlocker.ini       <- config
```

Check `xstore_unlocker.log` in the game folder to confirm hooks are working.

## Configuration

The INI file is created automatically on first launch if missing.

### [Settings]

| Key | Default | Description |
|-----|---------|-------------|
| `unlock_all` | 1 | Patches every DLC the game queries from the store as owned. Covers most DLCs. Does not cover unlisted DLCs the game never queries for. Set to 0 to disable. |
| `log_enabled` | 1 | Writes to `xstore_unlocker.log` and OutputDebugString. Set to 0 for silent operation. Errors always log regardless. |

### [Blacklist]

Store IDs to skip even when `unlock_all=1`. One per line.

```ini
[Blacklist]
9P4P5VWJWD6S=1
```

### [DLCs]

Some games have DLCs that exist in the store but are never queried at runtime. Promo items, hardware bundle exclusives, delisted content. The `unlock_all` option cannot reach these because the game never asks for them.

Add their Store IDs here. The unlocker injects fake owned products into the game's query results so it sees them as purchased.

Get Store IDs from [dbox.tools](https://dbox.tools) by searching for your game.

```ini
[DLCs]
9N6F78CNKF3L=1
9PNB6L2L9RW8=1
```

## Tested Games

| Game | Status | Notes |
|------|--------|-------|
| Forza Horizon 5 | Working | All DLCs including unlisted promo cars |
| Vampire Survivors | Working | All DLCs |
| DOOM: The Dark Ages | Working | All DLCs |
| Forza Horizon 6 | Working | All DLCs |

## Building From Source

Requires Visual Studio 2022+ with C++ desktop workload and CMake.

```
cd StoreUnlocker
mkdir build && cd build
cmake .. -G "Visual Studio 17 2022" -A x64
cmake --build . --config Release
```

Output: `build/Release/XGameRuntime.dll`

## Legal

This software is provided for educational and research purposes only. Use it at your own risk. The author is not responsible for any consequences of using this tool. Do not use this tool to obtain content you do not have the right to access. Support the developers by purchasing games and DLCs.
