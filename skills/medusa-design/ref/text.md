# Text

Docs: https://docs.medusajs.com/ui/components/text
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

Body copy.

## When to open this file

Open this file when the view needs `Text`, or a related control from the index row for `text`.

## Usage

```tsx
<Text>Text</Text>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `p` element and supports all of its props

- `size`: `xsmall` | `small` | `base` | `large` | `xlarge`
- `weight`: `regular` | `plus`
- `leading`: `normal` | `compact`

Flags:

- `asChild`

## Official examples

### `text-demo.tsx`

```tsx
import { Text } from "@medusajs/ui";

export default function TextDemo() {
  return <Text>Text</Text>;
}
```

### `text-fonts.tsx`

```tsx
import { Text } from "@medusajs/ui";

export default function TextFonts() {
  return (
    <div className="flex flex-col gap-y-2">
      <Text family="sans">Sans font</Text>
      <Text family="mono">Mono font</Text>
    </div>
  );
}
```

### `text-leading.tsx`

```tsx
import { Text } from "@medusajs/ui";

export default function TextLeading() {
  return (
    <div className="flex flex-col gap-y-2">
      <Text leading="normal">Normal leading</Text>
      <Text leading="compact">Compact leading</Text>
    </div>
  );
}
```

### `text-sizes.tsx`

```tsx
import { Text } from "@medusajs/ui";

export default function TextSizes() {
  return (
    <div className="flex flex-col gap-y-2">
      <Text size="base">Base size</Text>
      <Text size="large">Large size</Text>
      <Text size="xlarge">XLarge size</Text>
    </div>
  );
}
```

### `text-weights.tsx`

```tsx
import { Text } from "@medusajs/ui";

export default function TextWeights() {
  return (
    <div className="flex flex-col gap-y-2">
      <Text weight="regular">Regular weight</Text>
      <Text weight="plus">Plus weight</Text>
    </div>
  );
}
```

## Atlas

Subtitles, muted handles, fact labels. `size="small"` / `xsmall`, `className="text-ui-fg-subtle"` or `text-ui-fg-muted`.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
