---
title: "Desktop menus"
summary: "Compose native desktop menus and route standard window and editing commands."
category: "Overview"
order: 0
keywords: ["desktopmenus", "desktop", "menus"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# Desktop menus

desktopmenus supplies independent File/Edit/Window menu bars, platform primary shortcuts, and a narrow standard-command dispatcher. It has no Calls, CouchDB, or scraping dependency. The consuming application owns its Open, Save, Settings, and Help behavior.

## Platform support

Primary shortcuts use Cmd on macOS and Ctrl on Windows/Linux. Native editing routes through AppKit responders, Win32 edit messages, or GTK actions. Focused-control capabilities vary; ordinary Win32 EDIT has no redo queue and WebView editing is not guaranteed on Windows/Linux.

## Start reading

- [Getting started](getting-started.md) walks through installation and a complete example.
- [API reference](api.md) describes public symbols, state, errors, and ownership.

## Scope

Give each window its own MenuBar and attach it to Window.MenuBar. In MenuSelected handle application commands first, then pass remaining tags to PerformDesktopCommand. Call UpdateNativeEditingCommands from MenuNeedsUpdate. RegisterSystemMenus on activation and after replacing a menu bar. All UI operations belong on the main thread.
