---
title: "Desktop menus getting started"
summary: "Install desktopmenus and use its core API with explicit application ownership."
category: "Guides"
order: 10
keywords: ["desktopmenus", "desktop", "menus", "installation", "example"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Desktop menus getting started

## Install and import

Copy the sealed `desktopmenus-0.1.0.buipkg` archive into your project `packages/` directory, or install a source/registry dependency named `desktopmenus` using BPM and sync it. Then use `Import desktopmenus`. The archive includes this documentation as package resources.

Use a consuming source filename such as `main.bui`; a local module named exactly like the package shadows its import.

## Complete usage example

The example is compiler-checked as a complete Buoy module. Functions and Subs are intended to be called from the consuming app at the appropriate user action or lifecycle event.

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

## Integrate with the application

Give each window its own MenuBar and attach it to Window.MenuBar. In MenuSelected handle application commands first, then pass remaining tags to PerformDesktopCommand. Call UpdateNativeEditingCommands from MenuNeedsUpdate. RegisterSystemMenus on activation and after replacing a menu bar. All UI operations belong on the main thread.

Buoy already creates the native macOS About, Hide, Show All, and Quit app menu. RegisterSystemMenus preserves it and adds Services; it does not fabricate an empty settings dialog. Registration is repeated on the next default-mode run-loop turn because Buoy may install a window menu after activation. Nested calls from that tick retrigger the in-progress pass instead of restarting the firing timer. Set DesktopMenuState.RegisterEnabled to false to disable registration (Studio does this for --self-test).

## Handle failures and unsupported targets

AddMenuCommand throws InvalidArgumentException when the menu is Nil or the tag is empty, before creating or appending an item. Dispatch returns false for unsupported tags or a Nil window, except the quit tag: quit requests application termination even when the window is Nil. PerformNativeEdit accepts only undo, redo, cut, copy, paste, and select-all; arbitrary native selectors are not allowed. It returns false immediately for a Nil window.

Primary shortcuts use Cmd on macOS and Ctrl on Windows/Linux. Native editing routes through AppKit responders, Win32 edit messages, or GTK actions. Focused-control capabilities vary; ordinary Win32 EDIT has no redo queue and WebView editing is not guaranteed on Windows/Linux. A true editing result does not prove that text changed: on Windows it means a whitelisted message was sent to the focused widget, without checking the message result; macOS and Linux return the native action-dispatch result. Use the Boolean to route commands, not to verify an edit or persist document state.

UpdateNativeEditingCommands only updates the redo item on Windows, using the focused control's redo availability. It does not query every editing command or validate the focused control type. It does nothing on macOS/Linux or for a Nil window or missing MenuBar; consumers remain responsible for other command enablement.

Return to the [overview](index.md) or consult the [API reference](api.md).
