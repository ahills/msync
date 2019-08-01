# msync

This short sh script reads parameters for `xrandr` outputs from `~/.monitors`
and runs `xrandr` to set up connected monitors with the given parameters and
disable disconnected monitors.

## Usage

```
usage: msync [-v] [-d] -l
       msync [-v] [-d] [-n | -s] [monitors…]

options:
  -h  you're reading it
  -v  verbose output
  -d  debug output
  -n  dry run
  -l  list connected monitors
  -s  show matching configuration

if neither -l nor -s is specified, synchronizes monitor state with matching
configuration (see -s) using the arguments as randr monitor names on which to
operate, or all connected monitors if no arguments are specified (see -l)

configuration is read from $MSYNC_CONFIG, or ~/.monitors by default; syntax:
  * leading and trailing whitespace, as well as entire lines not matching any
    configuration, are ignored
  * each line specifies "name = value" sections, delimited by pipes (|),
    where the name is the monitor name and the value is the set of xrandr
    output arguments (e.g., --off, --mode 2560x1440, etc)
  * only lines with all active monitors (either connected or specified on the
    command line) will match
  * two special lines, starting with the words "before" and "after", can have
    a command after a $ symbol to be run just before and just after the
    xrandr command is run

example configuration file:

  these "comment" lines that are not valid configurations will be ignored

  enable eDP1 and set it as the primary display when it's the only one
  eDP1 = --primary

  when DP2 is present, make it the primary display, and disable eDP1
  eDP1 = --off | DP2 = --primary

  when DP1 is present, put it left of eDP1, unless DP2 is present
  eDP1 = --primary | DP1 = --left-of eDP1
  eDP1 = --primary | DP1 = --left-of DP2 | DP2 = --left-of eDP1

  ensure background is properly resized
  after $ ~/.fehbg

```

## Snippets

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
