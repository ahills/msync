# msync

This short sh script checks each available plug in `/sys/class/drm` on the
first card (card0). If `status` and `enabled` do not agree, `xrandr` is called
to take care of that. Additionally, if the `-a` ("all") flag is given as the
first command line argument, monitors that are both connected and enabled are
targeted by `xrandr`. Information about what is happening, including the full
`xrandr` command line, is printed to `stderr`.

To control monitor parameters, add the arguments to `xrandr` that would
typically follow the `--output` option for the display to the display's entry
in `~/.monitors`. The format is a simple `key = value`; when searching for
arguments for a display, only the line matching that display is read.

The following sample `~/.monitors` is what I use for connecting my laptop to a
single external display. I prefer to keep the laptop's display at its native
resolution, but let `xrandr` pick its favorite mode for the external display.

```INI
LVDS1 = --mode 1600x900
HDMI1 = --left-of LVDS1
```

Dynamic configuration is left as an exercise for the user.

If you are using [dwm](http://dwm.suckless.org) as your window manager, the
following `config.h` snippet may be useful:

```C
static const char *msynccmd[] = { "msync", NULL };
static const char *msyncAcmd[] = { "msync", "-a", NULL };
 
static Key keys[] = {
    /* ... */
    { MODKEY|ShiftMask,             XK_m,      spawn,          {.v = msynccmd } },
    { MODKEY|ShiftMask,             XK_t,      spawn,          {.v = msyncAcmd } },
    /* ... */
};
```

I also have `msync -a` at the start of my `~/.xsession`.

Copyright © 2015 Andrew Hills. See LICENSE for details.
