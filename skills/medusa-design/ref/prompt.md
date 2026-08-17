# Prompt

Docs: https://docs.medusajs.com/ui/components/prompt
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A confirm dialog.

## When to open this file

Open this file when the view needs `Prompt`, or a related control from the index row for `prompt`.

## Usage

```tsx
<Prompt>
  <Prompt.Trigger>Trigger</Prompt.Trigger>
  <Prompt.Content>
    <Prompt.Header>
      <Prompt.Title>Title</Prompt.Title>
      <Prompt.Description>Description</Prompt.Description>
    </Prompt.Header>
    <Prompt.Footer>
      <Prompt.Cancel>Cancel</Prompt.Cancel>
      <Prompt.Action>Delete</Prompt.Action>
    </Prompt.Footer>
  </Prompt.Content>
</Prompt>
```

## Official examples

### `prompt-demo.tsx`

```tsx
import { Button, Prompt } from "@medusajs/ui";

export default function PromptDemo() {
  return (
    <Prompt>
      <Prompt.Trigger asChild>
        <Button>Open</Button>
      </Prompt.Trigger>
      <Prompt.Content>
        <Prompt.Header>
          <Prompt.Title>Delete something</Prompt.Title>
          <Prompt.Description>Are you sure? This cannot be undone.</Prompt.Description>
        </Prompt.Header>
        <Prompt.Footer>
          <Prompt.Cancel>Cancel</Prompt.Cancel>
          <Prompt.Action>Delete</Prompt.Action>
        </Prompt.Footer>
      </Prompt.Content>
    </Prompt>
  );
}
```

### `prompt-confirmation.tsx`

```tsx
import { Button, Prompt } from "@medusajs/ui";

export default function PromptConfirmation() {
  return (
    <Prompt variant="confirmation">
      <Prompt.Trigger asChild>
        <Button>Open Confirmation</Button>
      </Prompt.Trigger>
      <Prompt.Content>
        <Prompt.Header>
          <Prompt.Title>Confirm Action</Prompt.Title>
          <Prompt.Description>
            Are you sure you want to proceed? This action can be undone.
          </Prompt.Description>
        </Prompt.Header>
        <Prompt.Footer>
          <Prompt.Cancel>Cancel</Prompt.Cancel>
          <Prompt.Action>Confirm</Prompt.Action>
        </Prompt.Footer>
      </Prompt.Content>
    </Prompt>
  );
}
```

## Atlas

Kit Overlays. Destructive confirm later. Do not invent a custom confirm dialog.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
