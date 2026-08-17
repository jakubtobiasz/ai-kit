# clx

Docs: https://docs.medusajs.com/ui/utils/clx
Import from `@medusajs/ui`.

`clx` is a utility function that adds class names to your components, with support for conditional classes and merging Tailwind CSS classes.

## When to open this file

Open this file when the view needs `clx`.

## Usage

```tsx
type BoxProps = {
  className?: string;
  children: React.ReactNode;
  mt: "sm" | "md" | "lg";
};

const Box = ({ className, children, mt }: BoxProps) => {
  return (
    <div
      className={clx(
        "flex items-center justify-center",
        {
          "mt-4": mt === "sm",
          "mt-8": mt === "md",
          "mt-12": mt === "lg",
        },
        className,
      )}
    >
      {children}
    </div>
  );
};
```

```tsx
clx("flex items-center justify-between");
```

```tsx
clx({
  "flex items-center justify-between": isFlex,
});
```

## Atlas

Shell and preview class merges. Import from `@medusajs/ui`.
