# desktopmenus · 0.1.0

Reusable desktop menu construction and native command dispatch, extracted from the
Calls app without any Calls, CouchDB or scraping dependencies. Install the sealed
package and `Import desktopmenus`.

| API | Purpose |
| --- | --- |
| `DesktopPrimaryShortcut(key)` | `Cmd+…` on macOS, `Ctrl+…` on Windows/Linux. |
| `AddMenuCommand(menu, caption, tag, shortcut = "") As MenuItem` | Creates/attaches an item and returns it for later enablement. A tag is required. |
| `StandardDesktopMenuBar()` | A fresh File/Edit/Window bar for one window. |
| `StandardEditMenu()` / `StandardWindowMenu()` | Composable menus when an app supplies its own bar. |
| `PerformDesktopCommand(window, tag) As Boolean` | Close, quit, native editing, minimize, zoom/restore and bring-all. False leaves app-specific tags unhandled. |
| `PerformNativeEdit(window, tag) As Boolean` | Only undo, redo, cut, copy, paste and select-all; arbitrary selectors are rejected. |
| `UpdateNativeEditingCommands(window)` | Disables unavailable Windows redo instead of offering a dead command. |
| `RegisterSystemMenus()` | Registers macOS Services, Window and Help menus, including native Help search. No-op elsewhere. |

Give **each window a separate bar** and attach it to `Window.MenuBar`. Handle
`Window.MenuSelected` by returning `PerformDesktopCommand(Me, item.Tag)` for
commands not handled by the app. Run `UpdateNativeEditingCommands(Me)` from
`MenuNeedsUpdate`. Application-specific New/Open/Save/Settings/Help commands belong
to the consuming app. No app menu or empty preferences dialog is fabricated.

Buoy creates the macOS About/Hide/Show All/Quit app menu. This package preserves it
and adds Services. The registration helper recognizes the English strip titles
`Window` and `Help`; localized menu-title matching is not implemented. Call it on
window activation and after replacing a bar. A one-shot timer repeats registration
on the next default-mode run-loop turn because Buoy's own key-window observer may
install its menu after `Activated`. Nested registration during that tick marks a
retrigger instead of restarting the firing timer. `DesktopMenuState.RegisterEnabled
= False` makes `RegisterSystemMenus` a no-op (Studio uses that for `--self-test`).
Call these UI APIs on the main thread.

Native editing uses AppKit's responder chain, Win32 edit messages and GTK text
actions. Actual editing capabilities depend on the focused control; ordinary
Win32 EDIT has no redo queue, while RichEdit does. WebView editing on Windows/Linux
is not guaranteed. The package does not override Alt+F4 or global OS shortcuts.

## Verification


Maintainers run the private validation suite before release. See the repository README for public build instructions.

