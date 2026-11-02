---
title: "desktopmenus typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: []
dependencies: []
symbols: ["AddMenuCommand", "DesktopMenuState", "DesktopPrimaryShortcut", "PerformDesktopCommand", "PerformNativeEdit", "RegisterSystemMenus", "StandardDesktopMenuBar", "StandardEditMenu", "StandardWindowMenu", "UpdateNativeEditingCommands"]
related: ["api.md", "getting-started.md", "index.md"]
---

# desktopmenus typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

### DesktopPrimaryShortcut

`Public Function DesktopPrimaryShortcut(key As String) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### AddMenuCommand

`Public Function AddMenuCommand(menu As Menu, caption As String, tag As String, shortcut As String = "") As MenuItem`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `menu` | `Menu` | `ByVal` | Required |
| `caption` | `String` | `ByVal` | Required |
| `tag` | `String` | `ByVal` | Required |
| `shortcut` | `String` | `ByVal` | `""` |

#### Returns

| Type |
| --- |
| `MenuItem` |

### PerformNativeEdit

`Public Function PerformNativeEdit(window As Window, command As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `window` | `Window` | `ByVal` | Required |
| `command` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### PerformDesktopCommand

`Public Function PerformDesktopCommand(window As Window, command As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `window` | `Window` | `ByVal` | Required |
| `command` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### RegisterSystemMenus

`Public Sub RegisterSystemMenus()`

No parameters.

Returns: no value.

### UpdateNativeEditingCommands

`Public Sub UpdateNativeEditingCommands(window As Window)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `window` | `Window` | `ByVal` | Required |

Returns: no value.

### StandardDesktopMenuBar

`Public Function StandardDesktopMenuBar() As MenuBar`

Build a fresh bar for each window; app-specific commands can be appended before attachment.

No parameters.

#### Returns

| Type |
| --- |
| `MenuBar` |

### StandardEditMenu

`Public Function StandardEditMenu() As Menu`

No parameters.

#### Returns

| Type |
| --- |
| `Menu` |

### StandardWindowMenu

`Public Function StandardWindowMenu() As Menu`

No parameters.

#### Returns

| Type |
| --- |
| `Menu` |
