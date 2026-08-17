# Textarea

Docs: https://docs.medusajs.com/ui/components/textarea
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A multi-line text field.

## When to open this file

Open this file when the view needs `Textarea`, or a related control from the index row for `textarea`.

## Usage

```tsx
<Textarea />
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `textarea` element and supports all of its props

## Official examples

### `textarea-demo.tsx`

```tsx
import { Textarea } from "@medusajs/ui";

export default function TextAreaDemo() {
  return <Textarea placeholder="Product description ..." />;
}
```

### `textarea-controlled.tsx`

```tsx
import { useState } from "react";
import { Textarea } from "@medusajs/ui";

export default function TextareaControlled() {
  const [value, setValue] = useState("");
  return (
    <div className="flex flex-col gap-y-2">
      <Textarea
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Write your feedback..."
        aria-label="Feedback"
      />
      <div className="text-ui-fg-muted txt-compact-small">{value.length} characters</div>
    </div>
  );
}
```

### `textarea-disabled.tsx`

```tsx
import { Textarea } from "@medusajs/ui";

export default function TextareaDisabled() {
  return <Textarea disabled placeholder="Disabled textarea" aria-label="Disabled textarea" />;
}
```

## Atlas

Create description and detail edit.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
