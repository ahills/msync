# msync

This short sh script reads parameters for `xrandr` outputs from `~/.monitors`
and runs `xrandr` to set up connected monitors with the given parameters and
disable disconnected monitors.

To control monitor parameters, add the arguments to `xrandr` that would
typically follow the `--output` option for the display to the display's entry
in `~/.monitors`. The format is a simple `key = value`; when searching for
arguments for a display, only the line matching that display is read.

Additionally, the `before` and `after` keys may be used to specify commands to
run before and after the entire process, respectively.

The following sample `~/.monitors` is what I use to connect my laptop (primary
display `eDP-1`) to two external DisplayPort displays (`DP-1` and `DP-2`), and
reset the background image on the new displays using `feh`:

```INI
after = ~/.fehbg

eDP-1 = --crtc 0
DP-1  = --above eDP-1   --mode 2560x1440 --crtc 1
DP-2  = --right-of DP-1 --mode 2560x1440 --crtc 2
```

Dynamic configuration is left as an exercise for the user.

If you are using [dwm](http://dwm.suckless.org) as your window manager, the
following `config.h` snippet may be useful:

```C
static const char *msynccmd[] = { "msync", NULL };
 
static Key keys[] = {
    /* ... */
    { MODKEY|ShiftMask,             XK_m,      spawn,          {.v = msynccmd } },
    /* ... */
};
```

I also have `msync` at the start of my `~/.xsession`.

Copyright © 2015-2017 Andrew Hills. See LICENSE for details.
