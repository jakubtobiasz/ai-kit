# StatusBadge

Docs: https://docs.medusajs.com/ui/components/status-badge
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A badge with a status color dot.

## When to open this file

Open this file when the view needs `StatusBadge`, or a related control from the index row for `status-badge`.

## Usage

```tsx
<StatusBadge color="green">Active</StatusBadge>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the span element and supports all of its props

- `color`: `green` | `red` | `blue` | `orange` | `grey` | `purple`

## Official examples

### `status-badge-demo.tsx`

```tsx
import { StatusBadge } from "@medusajs/ui";

export default function StatusBadgeDemo() {
  return <StatusBadge>Draft</StatusBadge>;
}
```

### `status-badge-all-colors.tsx`

```tsx
import { StatusBadge } from "@medusajs/ui";

export default function StatusBadgeAllColors() {
  return (
    <div className="flex flex-wrap gap-2">
      <StatusBadge color="green">Active</StatusBadge>
      <StatusBadge color="red">Error</StatusBadge>
      <StatusBadge color="orange">Pending</StatusBadge>
      <StatusBadge color="blue">Info</StatusBadge>
      <StatusBadge color="purple">Archived</StatusBadge>
      <StatusBadge color="grey">Draft</StatusBadge>
    </div>
  );
}
```

## Atlas

List and detail project status. Colors: active green, paused orange, archived grey.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
