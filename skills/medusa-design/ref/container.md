# Container

Docs: https://docs.medusajs.com/ui/components/container
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A card surface for grouped page content.

## When to open this file

Open this file when the view needs `Container`, or a related control from the index row for `container`.

## Usage

```tsx
<Container>Container</Container>
```

## Kit props (`@medusajs/ui` 4.2.1)

This component is based on the `div` element and supports all of its props

## Official examples

### `container-demo.tsx`

```tsx
import { Container } from "@medusajs/ui";

export default function ContainerDemo() {
  return <Container>Content</Container>;
}
```

### `container-layout.tsx`

```tsx
import { Container, Heading } from "@medusajs/ui";

export default function ContainerLayout() {
  return (
    <div className="flex h-full w-full">
      <div className="border-ui-border-base w-full max-w-[216px] border-r p-4">
        <Heading level="h3">Menubar</Heading>
      </div>
      <div className="flex w-full flex-col gap-y-2 px-8 pb-8 pt-6">
        <Container>
          <Heading>Section 1</Heading>
        </Container>
        <Container>
          <Heading>Section 2</Heading>
        </Container>
        <Container>
          <Heading>Section 3</Heading>
        </Container>
      </div>
    </div>
  );
}
```

## Atlas

Locked list and detail: `Container className="divide-y p-0"` with `px-6 py-4` section padding. Do not put list chrome edge-to-edge.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
