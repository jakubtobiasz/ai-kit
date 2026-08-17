# CodeBlock

Docs: https://docs.medusajs.com/ui/components/code-block
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A block for code snippets with copy and line numbers.

## When to open this file

Open this file when the view needs `CodeBlock`, or a related control from the index row for `code-block`.

## Usage

```tsx
<CodeBlock
  snippets={[
    {
      language: "tsx",
      label: "Label",
      code: 'import { useProduct } from "medusa-react"',
    },
  ]}
>
  <CodeBlock.Header />
  <CodeBlock.Body />
</CodeBlock>
```

```tsx
<TooltipProvider>
  <CodeBlock
    snippets={[
      {
        language: "tsx",
        label: "Label",
        code: 'import { useProduct } from "medusa-react"',
      },
    ]}
  >
    <CodeBlock.Header />
    <CodeBlock.Body />
  </CodeBlock>
</TooltipProvider>
```

## Kit props (`@medusajs/ui` 4.2.1)

The label of the code snippet's tab.

Flags:

- `hideCopy`
- `hideLineNumbers`

## Official examples

### `code-block-demo.tsx`

```tsx
import { CodeBlock, Label } from "@medusajs/ui";

const snippets = [
  {
    label: "cURL",
    language: "bash",
    code: `curl 'http://localhost:9000/store/products/PRODUCT_ID'\n -H 'x-publishable-key: YOUR_API_KEY'`,
    hideLineNumbers: true,
  },
  {
    label: "Medusa JS SDK",
    language: "jsx",
    code: `const { product } = await medusa.store.products.retrieve("prod_123")\nconsole.log(product.id)`,
  },
];

export default function CodeBlockDemo() {
  return (
    <div className="w-full">
      <CodeBlock snippets={snippets}>
        <CodeBlock.Header>
          <CodeBlock.Header.Meta>
            <Label weight={"plus"}>/product-detail.js</Label>
          </CodeBlock.Header.Meta>
        </CodeBlock.Header>
        <CodeBlock.Body />
      </CodeBlock>
    </div>
  );
}
```

### `code-block-no-copy.tsx`

```tsx
import { CodeBlock, Label } from "@medusajs/ui";

const snippets = [
  {
    label: "Medusa JS SDK",
    language: "jsx",
    code: `console.log("Hello, World!")`,
    hideCopy: true,
  },
];

export default function CodeBlockNoCopy() {
  return (
    <div className="w-full">
      <CodeBlock snippets={snippets}>
        <CodeBlock.Header>
          <CodeBlock.Header.Meta>
            <Label weight={"plus"}>/product-detail.js</Label>
          </CodeBlock.Header.Meta>
        </CodeBlock.Header>
        <CodeBlock.Body />
      </CodeBlock>
    </div>
  );
}
```

### `code-block-no-header.tsx`

```tsx
import { CodeBlock } from "@medusajs/ui";

const snippets = [
  {
    label: "Medusa JS SDK",
    language: "jsx",
    code: `console.log("Hello, World!")`,
  },
];

export default function CodeBlockNoHeader() {
  return (
    <div className="w-full">
      <CodeBlock snippets={snippets}>
        <CodeBlock.Body />
      </CodeBlock>
    </div>
  );
}
```

### `code-block-no-lines.tsx`

```tsx
import { CodeBlock, Label } from "@medusajs/ui";

const snippets = [
  {
    label: "Medusa JS SDK",
    language: "jsx",
    code: `console.log("Hello, ")\n\nconsole.log("World!")`,
    hideLineNumbers: true,
  },
];

export default function CodeBlockNoLines() {
  return (
    <div className="w-full">
      <CodeBlock snippets={snippets}>
        <CodeBlock.Header>
          <CodeBlock.Header.Meta>
            <Label weight={"plus"}>/product-detail.js</Label>
          </CodeBlock.Header.Meta>
        </CodeBlock.Header>
        <CodeBlock.Body />
      </CodeBlock>
    </div>
  );
}
```

### `code-block-single.tsx`

```tsx
import { CodeBlock, Label } from "@medusajs/ui";

const snippets = [
  {
    label: "Medusa JS SDK",
    language: "jsx",
    code: `console.log("Hello, World!")`,
  },
];

export default function CodeBlockSingle() {
  return (
    <div className="w-full">
      <CodeBlock snippets={snippets}>
        <CodeBlock.Header hideLabels={true}>
          <CodeBlock.Header.Meta>
            <Label weight={"plus"}>/product-detail.js</Label>
          </CodeBlock.Header.Meta>
        </CodeBlock.Header>
        <CodeBlock.Body />
      </CodeBlock>
    </div>
  );
}
```

## Atlas

Kit Data display. Tailwind v4 does not emit `grid-cols-[auto,1fr]`. `apps/web/src/app/globals.css` sets `pre.code-body.grid { grid-template-columns: auto 1fr; }`.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
