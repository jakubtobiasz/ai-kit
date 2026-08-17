# Copy

Docs: https://docs.medusajs.com/ui/components/copy
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A button that copies a string to the clipboard.

## When to open this file

Open this file when the view needs `Copy`, or a related control from the index row for `copy`.

## Usage

```tsx
<Copy content="yarn add @medusajs/ui" />
```

```tsx
<TooltipProvider>
  <Copy content="yarn add @medusajs/ui" />
</TooltipProvider>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `button` element and supports all of its props

- `variant`: `mini` | `default`

Flags:

- `asChild`

## Official examples

### `copy-demo.tsx`

```tsx
import { Copy } from "@medusajs/ui";

export default function CopyDemo() {
  return <Copy content="yarn add @medusajs/ui" />;
}
```

### `copy-as-child.tsx`

```tsx
import { PlusMini } from "@medusajs/icons";
import { Copy, IconButton, Text } from "@medusajs/ui";

export default function CopyAsChild() {
  return (
    <div className="flex items-center gap-x-2">
      <Text>Copy command</Text>
      <Copy content="yarn add @medusajs/ui" asChild>
        <IconButton>
          <PlusMini />
        </IconButton>
      </Copy>
    </div>
  );
}
```

### `copy-custom-display.tsx`

```tsx
import { Code, Copy } from "@medusajs/ui";

export default function CopyDemo() {
  return (
    <Copy content="yarn add @medusajs/ui">
      <Code>yarn add @medusajs/ui</Code>
    </Copy>
  );
}
```

## Atlas

Kit Actions. Needs `TooltipProvider` (preview shell and kit page already wrap).

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
