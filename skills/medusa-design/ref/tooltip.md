# Tooltip

Docs: https://docs.medusajs.com/ui/components/tooltip
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A hover tooltip. Needs TooltipProvider.

## When to open this file

Open this file when the view needs `Tooltip`, or a related control from the index row for `tooltip`.

## Usage

```tsx
<Tooltip content="Tooltip content">Trigger</Tooltip>
```

```tsx
<TooltipProvider>
  <Tooltip content="Tooltip content">Trigger</Tooltip>
</TooltipProvider>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the [Radix UI Tooltip](https://www.radix-ui.com/primitives/docs/components/tooltip) primitive.

## Official examples

### `tooltip-demo.tsx`

```tsx
import { InformationCircleSolid } from "@medusajs/icons";
import { Tooltip } from "@medusajs/ui";

export default function TooltipDemo() {
  return (
    <Tooltip content="The quick brown fox jumps over the lazy dog.">
      <InformationCircleSolid />
    </Tooltip>
  );
}
```

### `tooltip-maxwidth.tsx`

```tsx
import { Tooltip } from "@medusajs/ui";
import { InformationCircleSolid } from "@medusajs/icons";

export default function TooltipMaxWidth() {
  return (
    <Tooltip
      content="This is a very long tooltip message that demonstrates how you can use the maxWidth prop to control the width of the tooltip."
      maxWidth={320}
      className="text-center"
    >
      <InformationCircleSolid />
    </Tooltip>
  );
}
```

### `tooltip-sides.tsx`

```tsx
import { Tooltip } from "@medusajs/ui";
import { ArrowLongDown, ArrowLongLeft, ArrowLongRight, ArrowLongUp } from "@medusajs/icons";

export default function TooltipSides() {
  return (
    <div className="flex gap-8 items-center justify-center">
      <Tooltip content="Top" side="top">
        <ArrowLongUp />
      </Tooltip>
      <Tooltip content="Bottom" side="bottom">
        <ArrowLongDown />
      </Tooltip>
      <Tooltip content="Left" side="left">
        <ArrowLongLeft />
      </Tooltip>
      <Tooltip content="Right" side="right">
        <ArrowLongRight />
      </Tooltip>
    </div>
  );
}
```

## Atlas

Collapsed sidebar nav. Copy and some IconButtons. Wrap the tree in TooltipProvider.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
