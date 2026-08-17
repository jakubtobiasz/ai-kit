# CurrencyInput

Docs: https://docs.medusajs.com/ui/components/currency-input
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

An input for money amounts with a currency prefix.

## When to open this file

Open this file when the view needs `CurrencyInput`, or a related control from the index row for `currency-input`.

## Usage

```tsx
<CurrencyInput symbol="$" code="usd" />
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the input element and supports all of its props

- `size`: `small` | `base`

## Official examples

### `currency-input-demo.tsx`

```tsx
import { CurrencyInput } from "@medusajs/ui";

export default function CurrencyInputDemo() {
  return (
    <div className="max-w-[250px]">
      <CurrencyInput symbol="$" code="usd" />
    </div>
  );
}
```

### `currency-input-base.tsx`

```tsx
import { CurrencyInput } from "@medusajs/ui";

export default function CurrencyInputBase() {
  return (
    <div className="max-w-[250px]">
      <CurrencyInput size="base" symbol="$" code="usd" />
    </div>
  );
}
```

### `currency-input-controlled.tsx`

```tsx
import { useState } from "react";
import { CurrencyInput } from "@medusajs/ui";

export default function CurrencyInputControlled() {
  const [value, setValue] = useState<string | undefined>("");
  const formatValue = (val: string | undefined) => {
    if (!val) {
      return "";
    }
    return new Intl.NumberFormat("en-US", {
      style: "currency",
      currency: "USD",
    }).format(parseFloat(val));
  };
  return (
    <div className="max-w-[250px]">
      <CurrencyInput
        symbol="$"
        code="usd"
        value={value}
        onValueChange={setValue}
        aria-label="Amount"
      />
      <div className="mt-2 text-xs text-ui-fg-muted">Value: {formatValue(value)}</div>
    </div>
  );
}
```

### `currency-input-disabled.tsx`

```tsx
import { CurrencyInput } from "@medusajs/ui";

export default function CurrencyInputDisabled() {
  return (
    <div className="max-w-[250px]">
      <CurrencyInput symbol="€" code="eur" disabled value={"100"} aria-label="Amount" />
    </div>
  );
}
```

### `currency-input-error.tsx`

```tsx
import { useState } from "react";
import { CurrencyInput } from "@medusajs/ui";

export default function CurrencyInputError() {
  const [value, setValue] = useState<string | undefined>("0");
  const [touched, setTouched] = useState(false);
  const isError = touched && (!value || parseFloat(value) <= 0);
  return (
    <div className="max-w-[250px]">
      <CurrencyInput
        symbol="$"
        code="usd"
        value={value}
        onValueChange={(val) => setValue(val)}
        aria-label="Amount"
        aria-invalid={isError}
        onBlur={() => setTouched(true)}
        min={0.01}
      />
      {isError && (
        <div className="mt-2 text-xs text-ui-fg-error">Amount must be greater than 0</div>
      )}
    </div>
  );
}
```

### `currency-input-small.tsx`

```tsx
import { CurrencyInput } from "@medusajs/ui";

export default function CurrencyInputSmall() {
  return (
    <div className="max-w-[250px]">
      <CurrencyInput size="small" symbol="$" code="usd" />
    </div>
  );
}
```

## Atlas

Not used in locked CRUD previews.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
