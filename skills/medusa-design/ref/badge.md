# Badge

Docs: https://docs.medusajs.com/ui/components/badge
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A component for displaying labels or indicators in a badge style.

## When to open this file

Open this file when the view needs `Badge`, or a related control from the index row for `badge`.

## Usage

```tsx
<Badge>Badge</Badge>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `div` element and supports all of its props

- `color`: `green` | `red` | `blue` | `orange` | `grey` | `purple`
- `size`: `2xsmall` | `xsmall` | `small` | `base` | `large`

Flags:

- `asChild`

## Official examples

### `badge-demo.tsx`

```tsx
import { Badge } from "@medusajs/ui";

export default function BadgeDemo() {
  return <Badge>Badge</Badge>;
}
```

### `badge-all-colors.tsx`

```tsx
import { Badge } from "@medusajs/ui";

export default function BadgeAllColors() {
  return (
    <div className="flex gap-3">
      <Badge color="grey">Grey</Badge>
      <Badge color="red">Red</Badge>
      <Badge color="green">Green</Badge>
      <Badge color="blue">Blue</Badge>
      <Badge color="orange">Orange</Badge>
      <Badge color="purple">Purple</Badge>
    </div>
  );
}
```

### `badge-all-rounded.tsx`

```tsx
import { Badge } from "@medusajs/ui";

export default function BadgeAllRounded() {
  return (
    <div className="flex gap-3">
      <Badge rounded="base">Base Rounded</Badge>
      <Badge rounded="full">Full Rounded</Badge>
    </div>
  );
}
```

### `badge-all-sizes.tsx`

```tsx
import { Badge } from "@medusajs/ui";

export default function BadgeAllSizes() {
  return (
    <div className="flex gap-3 items-center">
      <Badge size="2xsmall">2xsmall</Badge>
      <Badge size="xsmall">xsmall</Badge>
      <Badge size="small">small</Badge>
      <Badge size="base">base</Badge>
      <Badge size="large">large</Badge>
    </div>
  );
}
```

## Atlas

Detail source facts. Prefer StatusBadge for Active / Paused / Archived.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
