# usePrompt

Docs: https://docs.medusajs.com/ui/hooks/use-prompt
Import from `@medusajs/ui`.

This hook returns a function that prompts the user to confirm an action.

## When to open this file

Open this file when the view needs `usePrompt`.

## Usage

```tsx
const dialog = usePrompt();
const actionFunction = async () => {
  const confirmed = await dialog({
    title: "Are you sure?",
    description: "Please confirm this action",
  });
};
```

## Atlas

Pairs with Prompt. Kit Overlays.
