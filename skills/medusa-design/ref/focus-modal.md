# FocusModal

Docs: https://docs.medusajs.com/ui/components/focus-modal
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A full-page modal over the main content.

## When to open this file

Open this file when the view needs `FocusModal`, or a related control from the index row for `focus-modal`.

## Usage

```tsx
<FocusModal>
  <FocusModal.Trigger>Trigger</FocusModal.Trigger>
  <FocusModal.Content>
    <FocusModal.Header>Title</FocusModal.Header>
    <FocusModal.Body>Content</FocusModal.Body>
  </FocusModal.Content>
</FocusModal>
```

## Kit props (`@medusajs/ui` 4.2.1)

@prop defaultOpen - Whether the modal is opened by default.

## Official examples

### `focus-modal-demo.tsx`

```tsx
import { Button, FocusModal, Heading, Input, Label, Text } from "@medusajs/ui";

export default function FocusModalDemo() {
  return (
    <FocusModal>
      <FocusModal.Trigger asChild>
        <Button>Edit Variant</Button>
      </FocusModal.Trigger>
      <FocusModal.Content>
        <FocusModal.Header>
          <FocusModal.Title>Edit Variant</FocusModal.Title>
        </FocusModal.Header>
        <FocusModal.Body className="flex flex-col items-center py-16">
          <div className="flex w-full max-w-lg flex-col gap-y-8">
            <div className="flex flex-col gap-y-1">
              <Heading>Create API key</Heading>
              <Text className="text-ui-fg-subtle">
                Create and manage API keys. You can create multiple keys to organize your
                applications.
              </Text>
            </div>
            <div className="flex flex-col gap-y-2">
              <Label htmlFor="key_name" className="text-ui-fg-subtle">
                Key name
              </Label>
              <Input id="key_name" placeholder="my_app" />
            </div>
          </div>
        </FocusModal.Body>
        <FocusModal.Footer>
          <Button>Save</Button>
        </FocusModal.Footer>
      </FocusModal.Content>
    </FocusModal>
  );
}
```

### `focus-modal-controlled.tsx`

```tsx
import { Button, FocusModal, Heading, Input, Label, Text } from "@medusajs/ui";
import { useState } from "react";

export default function FocusModalControlled() {
  const [open, setOpen] = useState(false);

  return (
    <div>
      <Button onClick={() => setOpen(true)}>Edit Variant</Button>
      <FocusModal open={open} onOpenChange={setOpen}>
        <FocusModal.Content>
          <FocusModal.Header>
            <FocusModal.Title>Edit Variant</FocusModal.Title>
          </FocusModal.Header>
          <FocusModal.Body className="flex flex-col items-center py-16">
            <div className="flex w-full max-w-lg flex-col gap-y-8">
              <div className="flex flex-col gap-y-1">
                <Heading>Create API key</Heading>
                <Text className="text-ui-fg-subtle">
                  Create and manage API keys. You can create multiple keys to organize your
                  applications.
                </Text>
              </div>
              <div className="flex flex-col gap-y-2">
                <Label htmlFor="key_name" className="text-ui-fg-subtle">
                  Key name
                </Label>
                <Input id="key_name" placeholder="my_app" />
              </div>
            </div>
          </FocusModal.Body>
          <FocusModal.Footer>
            <Button onClick={() => setOpen(false)}>Save</Button>
          </FocusModal.Footer>
        </FocusModal.Content>
      </FocusModal>
    </div>
  );
}
```

### `focus-modal-form.tsx`

```tsx
import { Button, FocusModal, Input, Label } from "@medusajs/ui";
import { useState } from "react";

export default function FocusModalForm() {
  const [open, setOpen] = useState(false);
  const [value, setValue] = useState("");

  function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    setOpen(false);
  }

  return (
    <div className="flex flex-col gap-2 items-center">
      <FocusModal open={open} onOpenChange={setOpen}>
        <FocusModal.Trigger asChild>
          <Button>Create Item</Button>
        </FocusModal.Trigger>
        <FocusModal.Content>
          <FocusModal.Header>
            <FocusModal.Title>Create Item</FocusModal.Title>
          </FocusModal.Header>
          <form onSubmit={handleSubmit} className="flex flex-col flex-1">
            <FocusModal.Body>
              <div className="p-6">
                <Label htmlFor="name">Name</Label>
                <Input
                  id="name"
                  value={value}
                  onChange={(e) => setValue(e.target.value)}
                  placeholder="Enter your name"
                />
              </div>
            </FocusModal.Body>
            <FocusModal.Footer>
              <Button type="submit">Submit</Button>
            </FocusModal.Footer>
          </form>
        </FocusModal.Content>
      </FocusModal>
      {value && <div className="text-ui-fg-muted">Form submitted with name: {value}</div>}
    </div>
  );
}
```

### `focus-modal-nested.tsx`

```tsx
import { Button, FocusModal } from "@medusajs/ui";

export default function NestedFocusModals() {
  return (
    <FocusModal>
      <FocusModal.Trigger asChild>
        <Button>Open Outer Modal</Button>
      </FocusModal.Trigger>
      <FocusModal.Content>
        <FocusModal.Header>
          <FocusModal.Title>Outer Modal</FocusModal.Title>
        </FocusModal.Header>
        <FocusModal.Body className="p-6 flex flex-col space-y-2">
          <p>This is the outer modal.</p>
          <FocusModal>
            <FocusModal.Trigger asChild>
              <Button variant="secondary">Open Nested Modal</Button>
            </FocusModal.Trigger>
            <FocusModal.Content>
              <FocusModal.Header>
                <FocusModal.Title>Nested Modal</FocusModal.Title>
              </FocusModal.Header>
              <FocusModal.Body className="p-6">
                <p>This is a nested focus modal for additional information.</p>
              </FocusModal.Body>
            </FocusModal.Content>
          </FocusModal>
        </FocusModal.Body>
      </FocusModal.Content>
    </FocusModal>
  );
}
```

## Atlas

Locked create: FocusModal plus ProgressTabs. See [views.md](views.md).

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
