# IconBadge

Docs: https://docs.medusajs.com/ui/components/icon-badge
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A badge that holds an icon.

## When to open this file

Open this file when the view needs `IconBadge`, or a related control from the index row for `icon-badge`.

## Usage

```tsx
<IconBadge>
  <BuildingTax />
</IconBadge>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `span` element and supports all of its props

- `size`: `base` | `large`

Flags:

- `asChild`

## Official examples

### `icon-badge-demo.tsx`

```tsx
import { BuildingTax } from "@medusajs/icons";
import { IconBadge } from "@medusajs/ui";

export default function IconBadgeDemo() {
  return (
    <IconBadge>
      <BuildingTax />
    </IconBadge>
  );
}
```

### `icon-badge-all-colors.tsx`

```tsx
import { IconBadge } from "@medusajs/ui";
import { BuildingTax } from "@medusajs/icons";

export default function IconBadgeAllColors() {
  return (
    <div className="flex gap-3">
      <IconBadge color="grey">
        <BuildingTax />
      </IconBadge>
      <IconBadge color="purple">
        <BuildingTax />
      </IconBadge>
      <IconBadge color="orange">
        <BuildingTax />
      </IconBadge>
      <IconBadge color="red">
        <BuildingTax />
      </IconBadge>
      <IconBadge color="blue">
        <BuildingTax />
      </IconBadge>
      <IconBadge color="green">
        <BuildingTax />
      </IconBadge>
    </div>
  );
}
```

### `icon-badge-all-sizes.tsx`

```tsx
import { IconBadge } from "@medusajs/ui";
import { BuildingTax } from "@medusajs/icons";

export default function IconBadgeAllSizes() {
  return (
    <div className="flex gap-3 items-center">
      <IconBadge size="base">
        <BuildingTax />
      </IconBadge>
      <IconBadge size="large">
        <BuildingTax />
      </IconBadge>
    </div>
  );
}
```

## Atlas

Kit Data display.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
