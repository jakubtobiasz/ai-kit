# Heading

Docs: https://docs.medusajs.com/ui/components/heading
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

Page and section titles.

## When to open this file

Open this file when the view needs `Heading`, or a related control from the index row for `heading`.

## Usage

```tsx
<Heading>A Title</Heading>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the heading element (`h1`, `h2`, etc...) depeneding on the specified level

## Official examples

### `heading-demo.tsx`

```tsx
import { Heading } from "@medusajs/ui";

export default function HeadingDemo() {
  return (
    <div className="flex flex-col items-center">
      <Heading level="h1">This is an H1 heading</Heading>
      <Heading level="h2">This is an H2 heading</Heading>
      <Heading level="h3">This is an H3 heading</Heading>
    </div>
  );
}
```

## Atlas

List card title `Projects`. Detail section titles `level="h2"`.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
