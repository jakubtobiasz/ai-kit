# InlineTip

Docs: https://docs.medusajs.com/ui/components/inline-tip
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

An inline tip or note.

## When to open this file

Open this file when the view needs `InlineTip`, or a related control from the index row for `inline-tip`.

## Usage

```tsx
<InlineTip label="This is a tip">
  <button>Hover me</button>
</InlineTip>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `div` element and supports all of its props.

- `variant`: `info` | `warning` | `error` | `success`

## Official examples

### `inline-tip-demo.tsx`

```tsx
import { InlineTip } from "@medusajs/ui";

export default function InlineTipDemo() {
  return (
    <InlineTip label="Tip">
      Medusa UI is a package of React components to be used in Medusa Admin customizations.
    </InlineTip>
  );
}
```

### `inline-tip-error.tsx`

```tsx
import { InlineTip } from "@medusajs/ui";

export default function InlineTipError() {
  return (
    <InlineTip label="Error" variant="error">
      An error occurred. Please try again.
    </InlineTip>
  );
}
```

### `inline-tip-success.tsx`

```tsx
import { InlineTip } from "@medusajs/ui";

export default function InlineTipSuccess() {
  return (
    <InlineTip label="Success" variant="success">
      Product created successfully!
    </InlineTip>
  );
}
```

### `inline-tip-warning.tsx`

```tsx
import { InlineTip } from "@medusajs/ui";

export default function InlineTipWarning() {
  return (
    <InlineTip label="Warning" variant="warning">
      This action cannot be undone.
    </InlineTip>
  );
}
```

## Atlas

Kit Feedback.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
