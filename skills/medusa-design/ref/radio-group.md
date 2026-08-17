# RadioGroup

Docs: https://docs.medusajs.com/ui/components/radio-group
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A set of radio options.

## When to open this file

Open this file when the view needs `RadioGroup`, or a related control from the index row for `radio-group`.

## Usage

```tsx
<RadioGroup>
  <RadioGroup.Item value="1" id="radio_1" />
  <RadioGroup.Item value="2" id="radio_2" />
  <RadioGroup.Item value="3" id="radio_3" />
</RadioGroup>
```

## Official examples

### `radio-group-demo.tsx`

```tsx
import { Label, RadioGroup } from "@medusajs/ui";

export default function RadioGroupDemo() {
  return (
    <RadioGroup>
      <div className="flex items-center gap-x-3">
        <RadioGroup.Item value="1" id="radio_1" />
        <Label htmlFor="radio_1" weight="plus">
          Radio 1
        </Label>
      </div>
      <div className="flex items-center gap-x-3">
        <RadioGroup.Item value="2" id="radio_2" />
        <Label htmlFor="radio_2" weight="plus">
          Radio 2
        </Label>
      </div>
      <div className="flex items-center gap-x-3">
        <RadioGroup.Item value="3" id="radio_3" />
        <Label htmlFor="radio_3" weight="plus">
          Radio 3
        </Label>
      </div>
    </RadioGroup>
  );
}
```

### `radio-group-choicebox.tsx`

```tsx
import { RadioGroup } from "@medusajs/ui";

export default function RadioGroupChoiceBox() {
  return (
    <RadioGroup defaultValue="option1">
      <RadioGroup.ChoiceBox
        value="option1"
        label="Option 1"
        description="This is the first option."
      />
      <RadioGroup.ChoiceBox
        value="option2"
        label="Option 2"
        description="This is the second option."
      />
      <RadioGroup.ChoiceBox
        value="option3"
        label="Option 3"
        description="This is the third option."
      />
    </RadioGroup>
  );
}
```

### `radio-group-controlled.tsx`

```tsx
import { Label, RadioGroup } from "@medusajs/ui";
import * as React from "react";

export default function RadioGroupControlled() {
  const [value, setValue] = React.useState("1");
  return (
    <div className="flex flex-col gap-2 items-center">
      <RadioGroup value={value} onValueChange={setValue}>
        <div className="flex items-center gap-x-3">
          <RadioGroup.Item value="1" id="radio_1_controlled" />
          <Label htmlFor="radio_1_controlled" weight="plus">
            Radio 1
          </Label>
        </div>
        <div className="flex items-center gap-x-3">
          <RadioGroup.Item value="2" id="radio_2_controlled" />
          <Label htmlFor="radio_2_controlled" weight="plus">
            Radio 2
          </Label>
        </div>
        <div className="flex items-center gap-x-3">
          <RadioGroup.Item value="3" id="radio_3_controlled" />
          <Label htmlFor="radio_3_controlled" weight="plus">
            Radio 3
          </Label>
        </div>
      </RadioGroup>
      <div className="txt-small text-ui-fg-muted">Selected value: {value}</div>
    </div>
  );
}
```

### `radio-group-descriptions.tsx`

```tsx
import { Label, RadioGroup, Text } from "@medusajs/ui";

export default function RadioGroupDescriptions() {
  return (
    <RadioGroup>
      <div className="flex items-start gap-x-3">
        <RadioGroup.Item value="1" id="radio_1_descriptions" />
        <div className="flex flex-col gap-y-0.5">
          <Label htmlFor="radio_1_descriptions" weight="plus">
            Radio 1
          </Label>
          <Text className="text-ui-fg-subtle">The quick brown fox jumps over the lazy dog.</Text>
        </div>
      </div>
      <div className="flex items-start gap-x-3">
        <RadioGroup.Item value="2" id="radio_2_descriptions" />
        <div className="flex flex-col gap-y-0.5">
          <Label htmlFor="radio_2_descriptions" weight="plus">
            Radio 2
          </Label>
          <Text className="text-ui-fg-subtle">The quick brown fox jumps over the lazy dog.</Text>
        </div>
      </div>
      <div className="flex items-start gap-x-3">
        <RadioGroup.Item value="3" id="radio_3_descriptions" />
        <div className="flex flex-col gap-y-0.5">
          <Label htmlFor="radio_3_descriptions" weight="plus">
            Radio 3
          </Label>
          <Text className="text-ui-fg-subtle">The quick brown fox jumps over the lazy dog.</Text>
        </div>
      </div>
    </RadioGroup>
  );
}
```

### `radio-group-disabled.tsx`

```tsx
import { Label, RadioGroup } from "@medusajs/ui";

export default function RadioGroupDisabled() {
  return (
    <RadioGroup>
      <div className="flex items-center gap-x-3">
        <RadioGroup.Item value="1" id="radio_1_disabled" />
        <Label htmlFor="radio_1_disabled" weight="plus">
          Radio 1
        </Label>
      </div>
      <div className="flex items-center gap-x-3">
        <RadioGroup.Item value="2" id="radio_2_disabled" />
        <Label htmlFor="radio_2_disabled" weight="plus">
          Radio 2
        </Label>
      </div>
      <div className="flex items-center gap-x-3">
        <RadioGroup.Item value="3" id="radio_3_disabled" disabled={true} />
        <Label htmlFor="radio_3_disabled" weight="plus">
          Radio 3
        </Label>
      </div>
    </RadioGroup>
  );
}
```

## Atlas

Kit Inputs.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
