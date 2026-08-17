# Command

Docs: https://docs.medusajs.com/ui/components/command
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A command palette surface.

## When to open this file

Open this file when the view needs `Command`, or a related control from the index row for `command`.

## Usage

```tsx
<Command>
  <code>yarn add @medusajs/ui</code>
</Command>
```

```tsx
<TooltipProvider>
  <Command>
    <code>yarn add @medusajs/ui</code>
    <Command.Copy content="yarn add @medusajs/ui" className="ml-auto" />
  </Command>
</TooltipProvider>
```

## Kit props (`@medusajs/ui` 4.2.1)

- `variant`: `mini` | `default`

Flags:

- `asChild`

## Official examples

### `command-demo.tsx`

```tsx
import { Badge, Command } from "@medusajs/ui";

export default function CommandDemo() {
  return (
    <div className="w-full">
      <Command>
        <Badge color="green">Get</Badge>
        <code>localhost:9000/store/products</code>
        <Command.Copy content="localhost:9000/store/products" className="ml-auto" />
      </Command>
    </div>
  );
}
```

### `command-bar-demo.tsx`

```tsx
import { Checkbox, CommandBar, Label, Text } from "@medusajs/ui";
import * as React from "react";

export default function CommandBarDemo() {
  const [selected, setSelected] = React.useState<boolean>(false);

  return (
    <div className="flex justify-center gap-y-2 flex-col">
      <div className="flex items-center gap-x-2">
        <Checkbox
          checked={selected}
          onCheckedChange={(checked) => setSelected(checked === true ? true : false)}
        />
        <Label>Item One</Label>
      </div>
      <div>
        <Text size="small" className="text-ui-fg-muted">
          Check the box to view the command bar
        </Text>
      </div>
      <CommandBar open={selected}>
        <CommandBar.Bar>
          <CommandBar.Value>1 selected</CommandBar.Value>
          <CommandBar.Seperator />
          <CommandBar.Command
            action={() => {
              alert("Delete");
            }}
            label="Delete"
            shortcut="d"
          />
          <CommandBar.Seperator />
          <CommandBar.Command
            action={() => {
              alert("Edit");
            }}
            label="Edit"
            shortcut="e"
          />
        </CommandBar.Bar>
      </CommandBar>
    </div>
  );
}
```

## Atlas

Not used in locked CRUD previews.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
