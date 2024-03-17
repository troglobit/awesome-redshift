Redshift for Awesome WM
=======================

This is a tiny Lua library to interface the [Awesome window manager][] with
[redshift][], a program that dims the screen to make it easier on your eyes,
especially at night.

What makes this any more useful than *gtk-redshift*, you might ask?
Well, it offers the following features:

* __Integration with awesome via lua wrapper functions.__ Toggling
  redshift with a keyboard shortcut?  Automatically undimming the screen
  when launching the photo editor? Now anything is possible.

* __Support for multiple monitor setups that don't use xrandr.__
  If you're multiheading graphics cards, this is a must-have.

* __Automatic, periodic brightness adjustments.__ (Well, redshift offers
  this out of the box, but it's worth mentioning the library takes care
  of this for you.)

The design of this library follows the KISS principle.  It comes with
convenient wrapper functions to do the dirty work for you.  It's up to
*you* to program your own keybindings, bells, and whistles.


Install
-------

First, you need to install [redshift][] and set up a configuration file
to help determine your location:

    ; ~/.config/redshift.conf
    [redshift]
    temp-day=6500K
    temp-night=5000
    transition=1
    ;gamma=0.8:0.7:0.8
    gamma=1.000:1.000:1.000
    ;location-provider=geoclue
    location-provider=manual
    adjustment-method=vidmode
    ;brightness=1.0:0.5

    ; The location provider and adjustment method settings
    ; are in their own sections.
    ; This is an example lat long for Västerås, Sweden
    [manual]
    lat=59.61617
    lon=16.55276

    ; In this example screen 1 is adjusted by vidmode. Note
    ; that the numbering starts from 0, so this is actually
    ; the second screen.
    ;[vidmode]
    ;screen=1

Second, in your awesome configuration directory:

```sh
    cd ~/.config/awesome/
    git clone git://github.com/troglobit/awesome-redshift.git redshift
```

In your `rc.lua`:

```lua
    local redshift = require("awesome-redshift")

    -- set binary path (optional)
    redshift.redshift = "/usr/bin/redshift"
    -- set additional redshift arguments (optional)
    redshift.options = "-c ~/.config/redshift.conf"
    -- 1 for dim, 0 for not dimmed
    redshift.init(1)
```


Usage
-----

Done! Now you have these functions at your disposal:

* `redshift.dim()`
* `redshift.undim()`
* `redshift.toggle()`

They're pretty self-explanatory ... but here's an example for your
`rc.lua`:

```lua
    -- Toggle redshift (night mode)
    globalkeys = gears.table.join(globalkeys,
            awful.key({ modkey }, "Pause",      function () redshift.toggle() end),
            awful.key({ modkey }, "d",          function () redshift.dim() end),
            awful.key({ modkey, "Shift" }, "d", function () redshift.undim() end)
    )
    root.keys(globalkeys)
```

The `Pause` key is used by the author to activate my the screensaver, so
Win+Pause is used here to toggle night mode with redshift.

[Awesome window manager]: http://awesome.naquadah.org
[redshift]: http://jonls.dk/redshift/
