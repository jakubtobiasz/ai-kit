# Avatar

Docs: https://docs.medusajs.com/ui/components/avatar
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A component for displaying user avatars with a fallback option.

## When to open this file

Open this file when the view needs `Avatar`, or a related control from the index row for `avatar`.

## Usage

```tsx
<Avatar src="https://avatars.githubusercontent.com/u/10656202?v=4" fallback="M" />
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the [Radix UI Avatar](https://www.radix-ui.com/primitives/docs/components/avatar) primitive.

- `variant`: `squared` | `rounded`
- `size`: `2xsmall` | `xsmall` | `small` | `base` | `large` | `xlarge`

## Official examples

### `avatar-demo.tsx`

```tsx
import { Avatar } from "@medusajs/ui";

export default function AvatarDemo() {
  return <Avatar src="https://avatars.githubusercontent.com/u/10656202?v=4" fallback="M" />;
}
```

### `avatar-accessible.tsx`

```tsx
import { Avatar } from "@medusajs/ui";

export default function AvatarAccessible() {
  return (
    <Avatar
      src="https://avatars.githubusercontent.com/u/10656202?v=4"
      fallback="M"
      aria-label="Medusa User"
    />
  );
}
```

### `avatar-custom-style.tsx`

```tsx
import { Avatar } from "@medusajs/ui";

export default function AvatarCustomStyle() {
  return (
    <Avatar
      src="https://avatars.githubusercontent.com/u/10656202?v=4"
      fallback="M"
      style={{
        boxShadow: "0 0 0 3px #fdba74, 0 1px 2px 0 rgba(0,0,0,0.05)",
        border: "none",
      }}
    />
  );
}
```

### `avatar-fallback.tsx`

```tsx
import { Avatar } from "@medusajs/ui";

export default function AvatarFallback() {
  return <Avatar fallback="JD" />;
}
```

### `avatar-sizes.tsx`

```tsx
import { Avatar } from "@medusajs/ui";

export default function AvatarSizes() {
  return (
    <div className="flex gap-4 items-center">
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        size="2xsmall"
      />
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        size="xsmall"
      />
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        size="small"
      />
      <Avatar src="https://avatars.githubusercontent.com/u/10656202?v=4" fallback="M" size="base" />
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        size="large"
      />
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        size="xlarge"
      />
    </div>
  );
}
```

### `avatar-variants.tsx`

```tsx
import { Avatar } from "@medusajs/ui";

export default function AvatarVariants() {
  return (
    <div className="flex gap-4">
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        variant="rounded"
      />
      <Avatar
        src="https://avatars.githubusercontent.com/u/10656202?v=4"
        fallback="M"
        variant="squared"
      />
    </div>
  );
}
```

## Atlas

Shell org and account triggers. See [views.md](views.md) and [org-menu-desktop.png](screenshots/org-menu-desktop.png).

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
