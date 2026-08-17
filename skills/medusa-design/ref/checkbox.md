# Checkbox

Docs: https://docs.medusajs.com/ui/components/checkbox
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A checkbox control.

## When to open this file

Open this file when the view needs `Checkbox`, or a related control from the index row for `checkbox`.

## Usage

```tsx
<Checkbox />
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the [Radix UI Checkbox](https://www.radix-ui.com/primitives/docs/components/checkbox) primitive.

## Official examples

### `checkbox-demo.tsx`

```tsx
import { Checkbox, Label } from "@medusajs/ui";

export default function CheckboxDemo() {
  return (
    <div className="flex items-center space-x-2">
      <Checkbox id="billing-shipping" />
      <Label htmlFor="billing-shipping">Billing address same as shipping address</Label>
    </div>
  );
}
```

### `checkbox-all-states.tsx`

```tsx
import { Checkbox, Label } from "@medusajs/ui";

export default function CheckboxAllStates() {
  return (
    <div className="flex flex-col gap-6">
      <div className="flex items-center gap-1">
        <Checkbox id="default" />
        <Label htmlFor="default">Default</Label>
      </div>
      <div className="flex items-center gap-1">
        <Checkbox id="checked" checked />
        <Label htmlFor="checked">Checked</Label>
      </div>
      <div className="flex items-center gap-1">
        <Checkbox id="disabled" disabled />
        <Label htmlFor="disabled">Disabled</Label>
      </div>
      <div className="flex items-center gap-1">
        <Checkbox id="indeterminate" checked="indeterminate" />
        <Label htmlFor="indeterminate">Indeterminate</Label>
      </div>
    </div>
  );
}
```

### `checkbox-controlled.tsx`

```tsx
import { Checkbox, CheckboxCheckedState, Label } from "@medusajs/ui";
import { useState } from "react";

export default function CheckboxControlled() {
  const [checked, setChecked] = useState<CheckboxCheckedState>(false);

  const handleToggle = () => {
    switch (checked) {
      case "indeterminate":
        setChecked(true);
        return;
      case true:
        setChecked(false);
        return;
      default:
        setChecked("indeterminate");
    }
  };

  return (
    <div className="flex flex-col gap-6 items-center">
      <span className="txt-small text-center w-3/4">
        The following checkbox will move from unchecked, to indeterminate, and finally checked each
        time you click it
      </span>
      <div className="flex items-center gap-2">
        <Checkbox id="controlled-checkbox" checked={checked} onCheckedChange={handleToggle} />
        <Label htmlFor="controlled-checkbox">
          Controlled Checkbox: (
          {checked === "indeterminate" ? "Indeterminate" : checked ? "Checked" : "Unchecked"})
        </Label>
      </div>
    </div>
  );
}
```

## Atlas

List row select. Kit Inputs. Public auth remember-me is visual-only and disabled.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
