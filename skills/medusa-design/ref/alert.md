# Alert

Docs: https://docs.medusajs.com/ui/components/alert
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A component for displaying important messages.

## When to open this file

Open this file when the view needs `Alert`, or a related control from the index row for `alert`.

## Usage

```tsx
<Alert>Here's a message</Alert>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the div element and supports all of its props

- `variant`: `error` | `success` | `warning` | `info`

Flags:

- `dismissible`

## Official examples

### `alert-demo.tsx`

```tsx
import { Alert } from "@medusajs/ui";

export default function AlertDemo() {
  return <Alert>You are viewing Medusa docs.</Alert>;
}
```

### `alert-dismissable.tsx`

```tsx
import { Alert } from "@medusajs/ui";

export default function AlertDismissable() {
  return <Alert dismissible={true}>You are viewing Medusa docs.</Alert>;
}
```

### `alert-error.tsx`

```tsx
import { Alert } from "@medusajs/ui";

export default function AlertError() {
  return <Alert variant="error">An error occured while updating data.</Alert>;
}
```

### `alert-success.tsx`

```tsx
import { Alert } from "@medusajs/ui";

export default function AlertSuccess() {
  return <Alert variant="success">Data updated successfully!</Alert>;
}
```

### `alert-warning.tsx`

```tsx
import { Alert } from "@medusajs/ui";

export default function AlertWarning() {
  return <Alert variant="warning">Be careful!</Alert>;
}
```

## Atlas

Kit Feedback. Form mutation errors on product routes still use `<p role="alert">` plus Hint. See `apps/web/DESIGN.md`.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
