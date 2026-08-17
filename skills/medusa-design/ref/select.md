# Select

Docs: https://docs.medusajs.com/ui/components/select
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A select field.

## When to open this file

Open this file when the view needs `Select`, or a related control from the index row for `select`.

## Usage

```tsx
<Select>
  <Select.Trigger>
    <Select.Value placeholder="Placeholder" />
  </Select.Trigger>
  <Select.Content>
    {items.map((item) => (
      <Select.Item key={item.value} value={item.value}>
        {item.label}
      </Select.Item>
    ))}
  </Select.Content>
</Select>
```

```tsx
<Select onValueChange={(value) => handleChange(value === "none" ? null : value)}>
  <Select.Trigger>
    <Select.Value placeholder="Placeholder" />
  </Select.Trigger>
  <Select.Content>
    <Select.Item value="none">None</Select.Item>
    {items.map((item) => (
      <Select.Item key={item.value} value={item.value}>
        {item.label}
      </Select.Item>
    ))}
  </Select.Content>
</Select>
```

## Kit props (`@medusajs/ui` 4.2.1)

- `size`: `base` | `small`

## Official examples

### `select-demo.tsx`

```tsx
import { Select } from "@medusajs/ui";

export default function SelectDemo() {
  return (
    <div className="w-[256px]">
      <Select>
        <Select.Trigger>
          <Select.Value placeholder="Select a currency" />
        </Select.Trigger>
        <Select.Content>
          {currencies.map((item) => (
            <Select.Item key={item.value} value={item.value}>
              {item.label}
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}

const currencies = [
  {
    value: "eur",
    label: "EUR",
  },
  {
    value: "usd",
    label: "USD",
  },
  {
    value: "dkk",
    label: "DKK",
  },
];
```

### `select-controlled.tsx`

```tsx
import { Select } from "@medusajs/ui";
import * as React from "react";

export default function SelectDemo() {
  const [value, setValue] = React.useState<string | undefined>();

  return (
    <div className="w-[256px]">
      <Select onValueChange={setValue} value={value}>
        <Select.Trigger>
          <Select.Value placeholder="Select a currency" />
        </Select.Trigger>
        <Select.Content>
          {currencies.map((item) => (
            <Select.Item key={item.value} value={item.value}>
              {item.label}
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}

const currencies = [
  {
    value: "eur",
    label: "EUR",
  },
  {
    value: "usd",
    label: "USD",
  },
  {
    value: "dkk",
    label: "DKK",
  },
];
```

### `select-disabled.tsx`

```tsx
import { Select } from "@medusajs/ui";

export default function SelectDemo() {
  return (
    <div className="w-[256px]">
      <Select disabled>
        <Select.Trigger>
          <Select.Value placeholder="Select a currency" />
        </Select.Trigger>
        <Select.Content>
          {currencies.map((item) => (
            <Select.Item key={item.value} value={item.value}>
              {item.label}
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}

const currencies = [
  {
    value: "eur",
    label: "EUR",
  },
  {
    value: "usd",
    label: "USD",
  },
  {
    value: "dkk",
    label: "DKK",
  },
];
```

### `select-grouped-items.tsx`

```tsx
import { Select } from "@medusajs/ui";

export default function SelectDemo() {
  return (
    <div className="w-[256px]">
      <Select>
        <Select.Trigger>
          <Select.Value placeholder="Select a currency" />
        </Select.Trigger>
        <Select.Content>
          {data.map((group) => (
            <Select.Group key={group.label}>
              <Select.Label>{group.label}</Select.Label>
              {group.items.map((item) => (
                <Select.Item key={item.value} value={item.value}>
                  {item.label}
                </Select.Item>
              ))}
            </Select.Group>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}

const data = [
  {
    label: "Shirts",
    items: [
      {
        value: "dress-shirt-solid",
        label: "Solid Dress Shirt",
      },
      {
        value: "dress-shirt-check",
        label: "Check Dress Shirt",
      },
    ],
  },
  {
    label: "T-Shirts",
    items: [
      {
        value: "v-neck",
        label: "V-Neck",
      },
      {
        value: "crew-neck",
        label: "Crew Neck",
      },
      {
        value: "henley",
        label: "Henley",
      },
    ],
  },
];
```

### `select-item-aligned.tsx`

```tsx
import { Select } from "@medusajs/ui";

export default function SelectItemAligned() {
  return (
    <div className="w-[256px]">
      <Select>
        <Select.Trigger>
          <Select.Value placeholder="Select a currency" />
        </Select.Trigger>
        <Select.Content position="item-aligned">
          {currencies.map((item) => (
            <Select.Item key={item.value} value={item.value}>
              {item.label}
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}

const currencies = [
  {
    value: "eur",
    label: "EUR",
  },
  {
    value: "usd",
    label: "USD",
  },
  {
    value: "dkk",
    label: "DKK",
  },
];
```

### `select-small.tsx`

```tsx
import { Select } from "@medusajs/ui";

export default function SelectSmall() {
  return (
    <div className="w-[256px]">
      <Select size="small">
        <Select.Trigger>
          <Select.Value placeholder="Select a currency" />
        </Select.Trigger>
        <Select.Content>
          {currencies.map((item) => (
            <Select.Item key={item.value} value={item.value}>
              {item.label}
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}

const currencies = [
  {
    value: "eur",
    label: "EUR",
  },
  {
    value: "usd",
    label: "USD",
  },
  {
    value: "dkk",
    label: "DKK",
  },
];
```

## Atlas

Create source kind. Kit Inputs.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
