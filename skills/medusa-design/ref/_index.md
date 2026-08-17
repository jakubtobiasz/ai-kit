# Medusa UI reference index

Read this file first. Then open only the matching `.md` file in this folder.

Docs source: [Medusa UI components](https://docs.medusajs.com/ui/components/copy). Installed kit: `@medusajs/ui` `4.2.1` and `@medusajs/icons` `2.19.0`.

## Locked Atlas views

For a simple screen, follow these previews. Do not invent a new layout unless the human asks.

Open [views.md](views.md). Screenshots live in [screenshots/](screenshots/).

| Need                              | File                                                                                           | Screenshot                                               |
| --------------------------------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Index / list / read / filters     | [table.md](table.md), [container.md](container.md), [dropdown-menu.md](dropdown-menu.md)       | [list-desktop.png](screenshots/list-desktop.png)         |
| Create / wizard / modal form      | [focus-modal.md](focus-modal.md), [progress-tabs.md](progress-tabs.md), [input.md](input.md)   | [create-desktop.png](screenshots/create-desktop.png)     |
| Detail / show / update drawer     | [container.md](container.md), [drawer.md](drawer.md), [status-badge.md](status-badge.md)       | [detail-desktop.png](screenshots/detail-desktop.png)     |
| Shell chrome / org menu / account | [dropdown-menu.md](dropdown-menu.md), [avatar.md](avatar.md), [icon-button.md](icon-button.md) | [org-menu-desktop.png](screenshots/org-menu-desktop.png) |
| Primitive lookup / variants       | [views.md](views.md) kit section                                                               | [kit-desktop.png](screenshots/kit-desktop.png)           |

Do not use [data-table.md](data-table.md) for the Atlas list. The locked list uses `Table` plus Add filter chips.

## Actions

| File                             | Primitive    | Open when you need                                      |
| -------------------------------- | ------------ | ------------------------------------------------------- |
| [button.md](button.md)           | `Button`     | Submit, Create, Export, Danger, loading, link-as-button |
| [icon-button.md](icon-button.md) | `IconButton` | Icon-only control. Collapse, bell, settings, row `...`  |
| [copy.md](copy.md)               | `Copy`       | Copy a string. Needs `TooltipProvider`                  |

## Inputs

| File                                   | Primitive       | Open when you need                       |
| -------------------------------------- | --------------- | ---------------------------------------- |
| [input.md](input.md)                   | `Input`         | Single-line text field                   |
| [textarea.md](textarea.md)             | `Textarea`      | Multi-line text field                    |
| [label.md](label.md)                   | `Label`         | Field label. Use `weight="plus"`         |
| [hint.md](hint.md)                     | `Hint`          | Field help or field error                |
| [checkbox.md](checkbox.md)             | `Checkbox`      | Boolean or row select                    |
| [switch.md](switch.md)                 | `Switch`        | Boolean switch                           |
| [radio-group.md](radio-group.md)       | `RadioGroup`    | One option from a small set              |
| [select.md](select.md)                 | `Select`        | One option from a list                   |
| [date-picker.md](date-picker.md)       | `DatePicker`    | Date field                               |
| [calendar.md](calendar.md)             | `Calendar`      | Raw calendar. Prefer DatePicker in forms |
| [currency-input.md](currency-input.md) | `CurrencyInput` | Money amount                             |
| [otp-input.md](otp-input.md)           | `OtpInput`      | One-time password digits                 |

## Feedback

| File                           | Primitive   | Open when you need             |
| ------------------------------ | ----------- | ------------------------------ |
| [alert.md](alert.md)           | `Alert`     | Page-level message             |
| [inline-tip.md](inline-tip.md) | `InlineTip` | Inline tip                     |
| [toast.md](toast.md)           | `toast()`   | Transient notice               |
| [toaster.md](toaster.md)       | `Toaster`   | Host for `toast()`. Mount once |
| [prompt.md](prompt.md)         | `Prompt`    | Confirm a destructive action   |
| [use-prompt.md](use-prompt.md) | `usePrompt` | Imperative confirm helper      |
| [skeleton.md](skeleton.md)     | `Skeleton`  | Loading placeholder            |

Product form errors still follow `apps/web/DESIGN.md`: field `Hint variant="error"`. Mutation errors use `<p role="alert">`.

## Overlays

| File                                 | Primitive      | Open when you need                   |
| ------------------------------------ | -------------- | ------------------------------------ |
| [focus-modal.md](focus-modal.md)     | `FocusModal`   | Create / full-page form              |
| [drawer.md](drawer.md)               | `Drawer`       | Edit a show-page section             |
| [dropdown-menu.md](dropdown-menu.md) | `DropdownMenu` | Menus, Add filter, row actions       |
| [popover.md](popover.md)             | `Popover`      | Small anchored panel                 |
| [tooltip.md](tooltip.md)             | `Tooltip`      | Hover label. Needs `TooltipProvider` |
| [command.md](command.md)             | `Command`      | Command palette                      |
| [command-bar.md](command-bar.md)     | `CommandBar`   | Bulk actions on selected rows        |

## Data display

| File                               | Primitive     | Open when you need                     |
| ---------------------------------- | ------------- | -------------------------------------- |
| [container.md](container.md)       | `Container`   | Card around a list or a detail section |
| [table.md](table.md)               | `Table`       | Locked index table                     |
| [data-table.md](data-table.md)     | `DataTable`   | Kit DataTable API. Not the Atlas list  |
| [heading.md](heading.md)           | `Heading`     | Page or section title                  |
| [text.md](text.md)                 | `Text`        | Body, subtitle, muted handle           |
| [badge.md](badge.md)               | `Badge`       | Neutral label                          |
| [status-badge.md](status-badge.md) | `StatusBadge` | Status with a color dot                |
| [icon-badge.md](icon-badge.md)     | `IconBadge`   | Icon inside a badge                    |
| [avatar.md](avatar.md)             | `Avatar`      | Org or user mark                       |
| [code.md](code.md)                 | `Code`        | Inline identifier                      |
| [code-block.md](code-block.md)     | `CodeBlock`   | YAML / snippet block                   |
| [kbd.md](kbd.md)                   | `Kbd`         | Shortcut chip such as `⌘K`             |
| [divider.md](divider.md)           | `Divider`     | Horizontal rule                        |

## Navigation

| File                                           | Primitive           | Open when you need         |
| ---------------------------------------------- | ------------------- | -------------------------- |
| [tabs.md](tabs.md)                             | `Tabs`              | In-page tabs               |
| [progress-tabs.md](progress-tabs.md)           | `ProgressTabs`      | Create wizard steps        |
| [progress-accordion.md](progress-accordion.md) | `ProgressAccordion` | Accordion with step status |

## Utils

| File                                       | Primitive        | Open when you need    |
| ------------------------------------------ | ---------------- | --------------------- |
| [clx.md](clx.md)                           | `clx`            | Merge class names     |
| [use-toggle-state.md](use-toggle-state.md) | `useToggleState` | Boolean toggle helper |

## Icons

There is no per-icon dump. Import from `@medusajs/icons`. Cite the Icons section in [views.md](views.md) and [kit-desktop.png](screenshots/kit-desktop.png).

Common Atlas icons: `CloudSolid`, `House`, `SquareTwoStack`, `ServerStack`, `Users`, `CogSixTooth`, `BellAlert`, `FunnelPlus`, `EllipsisHorizontal`, `ChevronUpDown`, `SidebarLeft`, `MagnifyingGlass`.
