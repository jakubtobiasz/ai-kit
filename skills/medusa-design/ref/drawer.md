# Drawer

Docs: https://docs.medusajs.com/ui/components/drawer
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A side panel over the current page.

## When to open this file

Open this file when the view needs `Drawer`, or a related control from the index row for `drawer`.

## Usage

```tsx
<Drawer>
  <Drawer.Trigger>Trigger</Drawer.Trigger>
  <Drawer.Content>
    <Drawer.Header>
      <Drawer.Title>Drawer Title</Drawer.Title>
    </Drawer.Header>
    <Drawer.Body>Body</Drawer.Body>
    <Drawer.Footer>Footer</Drawer.Footer>
  </Drawer.Content>
</Drawer>
```

## Kit props (`@medusajs/ui` 4.2.1)

The `Drawer.Content` component uses this component to wrap the drawer content.

## Official examples

### `drawer-demo.tsx`

```tsx
import { Button, Drawer, Text } from "@medusajs/ui";

export default function DrawerDemo() {
  return (
    <Drawer>
      <Drawer.Trigger asChild>
        <Button>Edit Variant</Button>
      </Drawer.Trigger>
      <Drawer.Content>
        <Drawer.Header>
          <Drawer.Title>Edit Variant</Drawer.Title>
        </Drawer.Header>
        <Drawer.Body className="p-4">
          <Text>This is where you edit the variant&apos;s details</Text>
        </Drawer.Body>
        <Drawer.Footer>
          <Drawer.Close asChild>
            <Button variant="secondary">Cancel</Button>
          </Drawer.Close>
          <Button>Save</Button>
        </Drawer.Footer>
      </Drawer.Content>
    </Drawer>
  );
}
```

### `drawer-form.tsx`

```tsx
import { useState } from "react";
import { Button, Drawer, Input, Label } from "@medusajs/ui";

export default function DrawerWithForm() {
  const [name, setName] = useState("");
  const [open, setOpen] = useState(false);
  const [submitted, setSubmitted] = useState(false);

  function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    setSubmitted(true);
    setOpen(false);
  }

  return (
    <div className="flex flex-col gap-2 items-center">
      <Drawer open={open} onOpenChange={setOpen}>
        <Drawer.Trigger asChild>
          <Button>Open Drawer</Button>
        </Drawer.Trigger>
        <Drawer.Content>
          <Drawer.Header>
            <Drawer.Title>Simple Form</Drawer.Title>
          </Drawer.Header>
          <form onSubmit={handleSubmit} className="flex flex-col flex-1">
            <Drawer.Body>
              <Label htmlFor="name">Name</Label>
              <Input
                id="name"
                value={name}
                onChange={(e) => setName(e.target.value)}
                placeholder="Enter your name"
              />
            </Drawer.Body>
            <Drawer.Footer>
              <Drawer.Close asChild>
                <Button variant="secondary" type="button">
                  Cancel
                </Button>
              </Drawer.Close>
              <Button type="submit">Submit</Button>
            </Drawer.Footer>
          </form>
        </Drawer.Content>
      </Drawer>
      {submitted && <div className="text-ui-fg-muted">Form submitted with name {name}</div>}
    </div>
  );
}
```

## Atlas

Locked detail edit: section Drawer from the show page. See [views.md](views.md).

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
