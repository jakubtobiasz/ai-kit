# Input

Docs: https://docs.medusajs.com/ui/components/input
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A text field.

## When to open this file

Open this file when the view needs `Input`, or a related control from the index row for `input`.

## Usage

```tsx
<Input placeholder="Placeholder" id="input-id" />
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `input` element and supports all of its props

- `size`: `small` | `base`

## Official examples

### `input-demo.tsx`

```tsx
import { Input } from "@medusajs/ui";

export default function InputDemo() {
  return (
    <div className="w-[250px]">
      <Input placeholder="Sales Channel Name" id="sales-channel-name" />
    </div>
  );
}
```

### `input-controlled.tsx`

```tsx
import { Input } from "@medusajs/ui";
import { useState } from "react";

export default function InputControlled() {
  const [value, setValue] = useState("");

  return (
    <div className="flex flex-col items-center gap-2">
      <Input
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Enter name"
        id="controlled-input"
      />
      {value && <span>Hello, {value}!</span>}
    </div>
  );
}
```

### `input-disabled.tsx`

```tsx
import { Input } from "@medusajs/ui";

export default function InputDisabled() {
  return (
    <div className="w-[250px]">
      <Input placeholder="Disabled" id="disabled-input" disabled />
    </div>
  );
}
```

### `input-error.tsx`

```tsx
import { Input } from "@medusajs/ui";

export default function InputError() {
  return (
    <div className="w-[250px]">
      <Input placeholder="Sales Channel Name" id="sales-channel-name" aria-invalid={true} />
    </div>
  );
}
```

### `input-password.tsx`

```tsx
import { Input } from "@medusajs/ui";

export default function InputPassword() {
  return (
    <div className="w-[250px]">
      <Input id="password" type="password" defaultValue="supersecret" />
    </div>
  );
}
```

### `input-search.tsx`

```tsx
import { Input } from "@medusajs/ui";

export default function InputSearch() {
  return (
    <div className="w-[250px]">
      <Input placeholder="Search" id="search-input" type="search" />
    </div>
  );
}
```

### `input-small.tsx`

```tsx
import { Input } from "@medusajs/ui";

export default function InputSmall() {
  return (
    <div className="w-[250px]">
      <Input placeholder="First name" id="first-name" size="small" />
    </div>
  );
}
```

## Atlas

Create and detail edit fields. Pair with Label and Hint. Recipe in `apps/web/DESIGN.md`.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
