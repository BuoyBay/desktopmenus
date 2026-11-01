---
title: "Desktop menus API"
summary: "Compose native desktop menus and route standard window and editing commands."
category: "Reference"
order: 20
keywords: ["desktopmenus", "desktop", "menus", "API", "lifecycle"]
symbols: ["DesktopPrimaryShortcut", "AddMenuCommand", "StandardDesktopMenuBar", "StandardEditMenu", "StandardWindowMenu", "PerformDesktopCommand", "PerformNativeEdit", "UpdateNativeEditingCommands", "RegisterSystemMenus", "DesktopMenuState"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# Desktop menus API

Import the `desktopmenus` package. Constructors below use the class name; invoke them with `New`. Listed members are public client APIs; native declarations and trampoline helpers are implementation details.

## Public surface

| Symbol | Contract |
| --- | --- |
| `DesktopPrimaryShortcut(key As String) As String` | Constructs a platform primary-key shortcut. |
| `AddMenuCommand(menu As Menu, caption As String, tag As String, shortcut As String = "") As MenuItem` | Creates, tags, and appends an item; returns it for later enablement. |
| `StandardDesktopMenuBar() As MenuBar` | Creates a fresh File/Edit/Window bar per call. |
| `StandardEditMenu() As Menu` | Creates the reusable standard editing menu. |
| `StandardWindowMenu() As Menu` | Creates the reusable standard window menu. |
| `PerformDesktopCommand(window As Window, command As String) As Boolean` | Dispatches close, quit, editing, minimize, zoom, and bring-all. Returns false for app-specific commands. |
| `PerformNativeEdit(window As Window, command As String) As Boolean` | Attempts a known editing action through the focused control. |
| `UpdateNativeEditingCommands(window As Window)` | Adjusts native editing command availability, including unsupported Windows redo. |
| `RegisterSystemMenus()` | Registers macOS Services, Window, and Help menus; no-op elsewhere. Recognition currently uses English Window/Help titles. |
| `DesktopMenuState.RegisterEnabled` | When false, `RegisterSystemMenus` is a no-op. Default true. |

## Ownership and timing

Give each window its own MenuBar and attach it to Window.MenuBar. In MenuSelected handle application commands first, then pass remaining tags to PerformDesktopCommand. Call UpdateNativeEditingCommands from MenuNeedsUpdate. RegisterSystemMenus on activation and after replacing a menu bar. All UI operations belong on the main thread.

## Errors and result interpretation

AddMenuCommand throws InvalidArgumentException when the menu is Nil or the tag is empty, before creating or appending an item. Dispatch returns false for unsupported tags or a Nil window, except the quit tag: quit requests application termination even when the window is Nil. PerformNativeEdit accepts only undo, redo, cut, copy, paste, and select-all; arbitrary native selectors are not allowed. It returns false immediately for a Nil window.

## Platform behavior

Primary shortcuts use Cmd on macOS and Ctrl on Windows/Linux. Native editing routes through AppKit responders, Win32 edit messages, or GTK actions. Focused-control capabilities vary; ordinary Win32 EDIT has no redo queue and WebView editing is not guaranteed on Windows/Linux. A true editing result does not prove that text changed: on Windows it means a whitelisted message was sent to the focused widget, without checking the message result; macOS and Linux return the native action-dispatch result. Use the Boolean to route commands, not to verify an edit or persist document state.

UpdateNativeEditingCommands only updates the redo item on Windows, using the focused control's redo availability. It does not query every editing command or validate the focused control type. It does nothing on macOS/Linux or for a Nil window or missing MenuBar; consumers remain responsible for other command enablement.

See the [complete usage example](getting-started.md) and the [package overview](index.md).


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
