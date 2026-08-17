# Button

Docs: https://docs.medusajs.com/ui/components/button
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A component for rendering buttons using Medusa's design system.

## When to open this file

Open this file when the view needs `Button`, or a related control from the index row for `button`.

## Usage

```tsx
<Button>Button</Button>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `button` element and supports all of its props

- `variant`: `primary` | `transparent` | `secondary` | `danger`
- `size`: `small` | `base` | `large` | `xlarge`

Flags:

- `isLoading`
- `asChild`

## Official examples

### `button-demo.tsx`

```tsx
import { Button } from "@medusajs/ui";

export default function ButtonDemo() {
  return <Button>Button</Button>;
}
```

### `button-all-sizes.tsx`

```tsx
import { Button } from "@medusajs/ui";

export default function ButtonAllSizes() {
  return (
    <div className="flex gap-4 items-center">
      <Button size="small">Small</Button>
      <Button size="base">Base</Button>
      <Button size="large">Large</Button>
      <Button size="xlarge">XLarge</Button>
    </div>
  );
}
```

### `button-all-variants.tsx`

```tsx
import { Button } from "@medusajs/ui";

export default function ButtonAllVariants() {
  return (
    <div className="flex gap-4">
      <Button variant="primary">Primary</Button>
      <Button variant="secondary">Secondary</Button>
      <Button variant="transparent">Transparent</Button>
      <Button variant="danger">Danger</Button>
    </div>
  );
}
```

### `button-as-link.tsx`

```tsx
import { Button } from "@medusajs/ui";

export default function ButtonAsLink() {
  return (
    <Button asChild>
      <a href="https://medusajs.com" target="_blank" rel="noopener noreferrer">
        Open Medusa Website
      </a>
    </Button>
  );
}
```

### `button-loading.tsx`

```tsx
import { Button } from "@medusajs/ui";

export default function ButtonLoading() {
  return <Button isLoading={true}>Button</Button>;
}
```

### `button-with-icon.tsx`

```tsx
import { PlusMini } from "@medusajs/icons";
import { Button } from "@medusajs/ui";

export default function ButtonWithIcon() {
  return (
    <Button>
      Button <PlusMini />
    </Button>
  );
}
```

## Atlas

List header: Export `variant="secondary"`, Create `variant="primary"`, both `size="small"`. Kit Actions shows all variants.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
