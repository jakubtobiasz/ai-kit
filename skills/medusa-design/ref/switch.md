# Switch

Docs: https://docs.medusajs.com/ui/components/switch
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A boolean switch.

## When to open this file

Open this file when the view needs `Switch`, or a related control from the index row for `switch`.

## Usage

```tsx
<Switch />
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the [Radix UI Switch](https://www.radix-ui.com/primitives/docs/components/switch) primitive.

- `size`: `small` | `base`

## Official examples

### `switch-demo.tsx`

```tsx
import { Label, Switch } from "@medusajs/ui";

export default function SwitchDemo() {
  return (
    <div className="flex items-center gap-x-2">
      <Switch id="manage-inventory" />
      <Label htmlFor="manage-inventory">Manage Inventory</Label>
    </div>
  );
}
```

### `switch-all-sizes.tsx`

```tsx
import { Label, Switch } from "@medusajs/ui";

export default function SwitchAllSizes() {
  return (
    <div className="flex flex-col gap-y-4">
      <div className="flex items-center gap-x-2">
        <Switch id="switch-small" size="small" />
        <Label htmlFor="switch-small" size="small">
          Small switch
        </Label>
      </div>
      <div className="flex items-center gap-x-2">
        <Switch id="switch-base" size="base" />
        <Label htmlFor="switch-base" size="base">
          Base switch
        </Label>
      </div>
    </div>
  );
}
```

### `switch-controlled.tsx`

```tsx
import { useState } from "react";
import { Label, Switch } from "@medusajs/ui";

export default function SwitchControlled() {
  const [checked, setChecked] = useState(false);

  return (
    <div className="flex flex-col gap-2">
      <div className="flex items-center gap-x-2">
        <Switch id="manage-inventory-controlled" checked={checked} onCheckedChange={setChecked} />
        <Label htmlFor="manage-inventory-controlled">Manage Inventory</Label>
      </div>
      <div className="txt-small text-ui-fg-muted">
        {checked ? "You are managing inventory" : "You are not managing inventory"}
      </div>
    </div>
  );
}
```

### `switch-disabled.tsx`

```tsx
import { Label, Switch } from "@medusajs/ui";

export default function SwitchDisabled() {
  return (
    <div className="flex items-center gap-x-2">
      <Switch id="manage-inventory-disabled" disabled={true} />
      <Label htmlFor="manage-inventory-disabled">Manage Inventory</Label>
    </div>
  );
}
```

## Atlas

Kit Inputs.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
