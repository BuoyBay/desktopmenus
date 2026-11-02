# Native menus and shortcuts

Reusable desktop menu construction and native command dispatch, extracted from the
Calls app without any Calls, CouchDB or scraping dependencies. Install the sealed
package and `Import desktopmenus`.

Buoy creates the macOS About/Hide/Show All/Quit app menu. This package preserves it
and adds Services. The registration helper recognizes the English strip titles
`Window` and `Help`; localized menu-title matching is not implemented. Call it on
window activation and after replacing a bar. A one-shot timer repeats registration
on the next default-mode run-loop turn because Buoy's own key-window observer may
install its menu after `Activated`. Nested registration during that tick marks a
retrigger instead of restarting the firing timer. `DesktopMenuState.RegisterEnabled
= False` makes `RegisterSystemMenus` a no-op (Studio uses that for `--self-test`).
Call these UI APIs on the main thread.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add desktopmenus --git https://github.com/BuoyBay/desktopmenus.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

```basic
Import desktopmenus

Sub AttachStandardMenus(window As Window)
    window.MenuBar = StandardDesktopMenuBar()
    RegisterSystemMenus()
End Sub

Function DispatchStandardCommand(window As Window, item As MenuItem) As Boolean
    Return PerformDesktopCommand(window, item.Tag)
End Function
```

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer desktopmenus
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
