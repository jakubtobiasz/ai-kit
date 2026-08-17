# CommandBar

Docs: https://docs.medusajs.com/ui/components/command-bar
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A bar of bulk actions for selected table rows.

## When to open this file

Open this file when the view needs `CommandBar`, or a related control from the index row for `command-bar`.

## Usage

```tsx
<CommandBar open={open}>
  <CommandBar.Bar>
    <CommandBar.Value>{count} selected</CommandBar.Value>
    <CommandBar.Seperator />
    <CommandBar.Command action={onDelete} label="Delete" shortcut="d" />
    <CommandBar.Seperator />
    <CommandBar.Command action={onEdit} label="Edit" shortcut="e" />
  </CommandBar.Bar>
</CommandBar>
```

## Official examples

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

Kit Overlays demo. List uses checkboxes without a command bar for now.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
