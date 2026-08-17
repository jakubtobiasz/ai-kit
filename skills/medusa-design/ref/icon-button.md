# IconButton

Docs: https://docs.medusajs.com/ui/components/icon-button
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A button that holds only an icon.

## When to open this file

Open this file when the view needs `IconButton`, or a related control from the index row for `icon-button`.

## Usage

```tsx
<IconButton>
  <Plus />
</IconButton>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `button` element and supports all of its props

- `variant`: `primary` | `transparent`
- `size`: `2xsmall` | `xsmall` | `small` | `base` | `large` | `xlarge`

Flags:

- `isLoading`
- `asChild`

## Official examples

### `icon-button-demo.tsx`

```tsx
import { PlusMini } from "@medusajs/icons";
import { IconButton } from "@medusajs/ui";

export default function IconButtonDemo() {
  return (
    <IconButton>
      <PlusMini />
    </IconButton>
  );
}
```

### `icon-button-all-sizes.tsx`

```tsx
import { IconButton } from "@medusajs/ui";
import { PlusMini } from "@medusajs/icons";

export default function IconButtonAllSizes() {
  return (
    <div className="flex gap-2 items-center">
      <IconButton size="2xsmall">
        <PlusMini />
      </IconButton>
      <IconButton size="xsmall">
        <PlusMini />
      </IconButton>
      <IconButton size="small">
        <PlusMini />
      </IconButton>
      <IconButton size="base">
        <PlusMini />
      </IconButton>
      <IconButton size="large">
        <PlusMini />
      </IconButton>
      <IconButton size="xlarge">
        <PlusMini />
      </IconButton>
    </div>
  );
}
```

### `icon-button-all-variants.tsx`

```tsx
import { IconButton } from "@medusajs/ui";
import { PlusMini } from "@medusajs/icons";

export default function IconButtonAllVariants() {
  return (
    <div className="flex gap-2">
      <IconButton variant="primary">
        <PlusMini />
      </IconButton>
      <IconButton variant="transparent">
        <PlusMini />
      </IconButton>
    </div>
  );
}
```

### `icon-button-disabled.tsx`

```tsx
import { PlusMini } from "@medusajs/icons";
import { IconButton } from "@medusajs/ui";

export default function IconButtonDisabled() {
  return (
    <IconButton disabled>
      <PlusMini />
    </IconButton>
  );
}
```

### `icon-button-loading.tsx`

```tsx
import { PlusMini } from "@medusajs/icons";
import { IconButton } from "@medusajs/ui";

export default function IconButtonLoading() {
  return (
    <IconButton isLoading className="relative">
      <PlusMini />
    </IconButton>
  );
}
```

## Atlas

Shell collapse, settings, bell. List row ellipsis. Transparent + small on tables.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
