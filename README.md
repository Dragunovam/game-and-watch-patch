Custom firmware for the Game and Watch: Super Mario Bros. console.


[![Click to play demo](https://thumbs.gfycat.com/UntriedMajesticAfricancivet-mobile.jpg)](https://gfycat.com/untriedmajesticafricancivet)

# What is this?
This repo contains custom code as well as a patching utility to add additional
functionality to the stock Game and Watch firmware.

# Features
* Works correctly with [retro-go](https://github.com/kbeckmann/game-and-watch-retro-go) in internal flash bank 2.
* Press button combination (`LEFT` + `A` + `GAME`) to launch retro-go from internal flash bank 2.

# Usage
Place your `internal_flash_backup.bin` and `flash_backup.bin` in the root of this
repo. To extract these from your console, see the [game and watch backup repo](https://github.com/ghidraninja/game-and-watch-backup)

Install python dependencies via:
```
pip3 install -r requirements.txt
```

Download STM32 Driver files:
```
make download_sdk
```

To just build and flash, run:
```
make flash
```

# Advanced usage
Other potentially useful make targets:

```
make clean
make patch  # Generates the patched bin at build/internal_flash_patched.bin, but doesn't flash
make flash_stock_int
make flash_stock_ext
make flash_patch_int
```

# TODO
* Figure out safe place in RAM to store global/static variables. The current
  configuration described in the linker file is unsafe, but currently we have
  no global/static variables.
* Maybe slim external flash ROM (remove easter eggs, ROMs, etc) to make room
  for more homebrew.

# Development
Main stages to developing a feature:
1. Find a place to take control in the stock rom (usually function calls).
2. Add the stock function and its address to `Core/Inc/stock_firmware.h`.
3. Implement your own function, possibly in `Core/Src/main.c`. There's a good chance your custom function will call the function in (2). You will also probably have to add `-Wl,--undefined=my_custom_function` to `LDFLAGS` in the Makefile so that it doesn't get optimized out as unreachable code.
4. Add a patch definition to `patches/patches.py`.

# Journal
This is my first time ever developing patches for a closed source binary. [I documented my journey in hopes that it helps other people](docs/journal.md). If you have any recommendations, tips, tricks, or anything like that, please leave a github issue and I'll update the documentation!
