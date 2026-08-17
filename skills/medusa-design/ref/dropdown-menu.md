# DropdownMenu

Docs: https://docs.medusajs.com/ui/components/dropdown-menu
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A menu of actions anchored to a trigger.

## When to open this file

Open this file when the view needs `DropdownMenu`, or a related control from the index row for `dropdown-menu`.

## Usage

```tsx
<DropdownMenu>
  <DropdownMenu.Trigger>Trigger</DropdownMenu.Trigger>
  <DropdownMenu.Content>
    <DropdownMenu.Item>Edit</DropdownMenu.Item>
    <DropdownMenu.Item>Add</DropdownMenu.Item>
    <DropdownMenu.Item>Delete</DropdownMenu.Item>
  </DropdownMenu.Content>
</DropdownMenu>
```

## Official examples

### `dropdown-menu-demo.tsx`

```tsx
import { EllipsisHorizontal, PencilSquare, Plus, Trash } from "@medusajs/icons";
import { DropdownMenu, IconButton } from "@medusajs/ui";

export default function DropdownMenuDemo() {
  return (
    <DropdownMenu>
      <DropdownMenu.Trigger asChild>
        <IconButton>
          <EllipsisHorizontal />
        </IconButton>
      </DropdownMenu.Trigger>
      <DropdownMenu.Content>
        <DropdownMenu.Item className="gap-x-2">
          <PencilSquare className="text-ui-fg-subtle" />
          Edit
        </DropdownMenu.Item>
        <DropdownMenu.Item className="gap-x-2">
          <Plus className="text-ui-fg-subtle" />
          Add
        </DropdownMenu.Item>
        <DropdownMenu.Separator />
        <DropdownMenu.Item className="gap-x-2">
          <Trash className="text-ui-fg-subtle" />
          Delete
        </DropdownMenu.Item>
      </DropdownMenu.Content>
    </DropdownMenu>
  );
}
```

### `dropdown-menu-disabled-icons.tsx`

```tsx
import { DropdownMenu, IconButton } from "@medusajs/ui";
import { Trash, BarsThree } from "@medusajs/icons";

export default function DropdownMenuDisabledAndIcons() {
  return (
    <DropdownMenu>
      <DropdownMenu.Trigger asChild>
        <IconButton>
          <BarsThree />
        </IconButton>
      </DropdownMenu.Trigger>
      <DropdownMenu.Content>
        <DropdownMenu.Item>Edit</DropdownMenu.Item>
        <DropdownMenu.Item disabled>
          <Trash className="mr-2" />
          Delete
        </DropdownMenu.Item>
      </DropdownMenu.Content>
    </DropdownMenu>
  );
}
```

### `dropdown-menu-shortcuts.tsx`

```tsx
import { useEffect, useCallback } from "react";
import { DropdownMenu, IconButton, toast, Toaster } from "@medusajs/ui";
import { Keyboard } from "@medusajs/icons";

function getOsShortcut() {
  const isMacOs =
    typeof navigator !== "undefined"
      ? navigator.userAgent.toLowerCase().indexOf("mac") !== 0
      : true;

  return isMacOs ? "⌘" : "Ctrl";
}

export default function DropdownMenuWithShortcuts() {
  const osShortcut = getOsShortcut();
  const handleEdit = useCallback(() => {
    toast.success("Success", {
      description: "Edit shortcut triggered!",
    });
  }, []);

  const handleDelete = useCallback(() => {
    toast.success("Success", {
      description: "Delete shortcut triggered!",
    });
  }, []);

  useEffect(() => {
    function handleKeydown(e: KeyboardEvent) {
      if (e.metaKey && e.key.toLowerCase() === "e") {
        e.preventDefault();
        handleEdit();
      }
      if (e.metaKey && e.key.toLowerCase() === "d") {
        e.preventDefault();
        handleDelete();
      }
    }
    window.addEventListener("keydown", handleKeydown);
    return () => window.removeEventListener("keydown", handleKeydown);
  }, [handleEdit, handleDelete]);

  return (
    <>
      <DropdownMenu>
        <DropdownMenu.Trigger asChild>
          <IconButton>
            <Keyboard />
          </IconButton>
        </DropdownMenu.Trigger>
        <DropdownMenu.Content>
          <DropdownMenu.Item onSelect={handleEdit}>
            Edit
            <DropdownMenu.Shortcut>{osShortcut}E</DropdownMenu.Shortcut>
          </DropdownMenu.Item>
          <DropdownMenu.Item onSelect={handleDelete}>
            Delete
            <DropdownMenu.Shortcut>{osShortcut}D</DropdownMenu.Shortcut>
          </DropdownMenu.Item>
        </DropdownMenu.Content>
      </DropdownMenu>
      <Toaster />
    </>
  );
}
```

### `dropdown-menu-sorting.tsx`

```tsx
import { EllipsisHorizontal } from "@medusajs/icons";
import { DropdownMenu, IconButton } from "@medusajs/ui";
import React from "react";

type SortingState = "asc" | "desc" | "alpha" | "alpha-reverse" | "none";

export default function DropdownMenuSorting() {
  const [sort, setSort] = React.useState<SortingState>("none");

  return (
    <div className="flex flex-col items-center gap-y-2">
      <DropdownMenu>
        <DropdownMenu.Trigger asChild>
          <IconButton>
            <EllipsisHorizontal />
          </IconButton>
        </DropdownMenu.Trigger>
        <DropdownMenu.Content className="w-[300px]">
          <DropdownMenu.RadioGroup value={sort} onValueChange={(v) => setSort(v as SortingState)}>
            <DropdownMenu.RadioItem value="none">No Sorting</DropdownMenu.RadioItem>
            <DropdownMenu.Separator />
            <DropdownMenu.RadioItem value="alpha">
              Alphabetical
              <DropdownMenu.Hint>A-Z</DropdownMenu.Hint>
            </DropdownMenu.RadioItem>
            <DropdownMenu.RadioItem value="alpha-reverse">
              Reverse Alphabetical
              <DropdownMenu.Hint>Z-A</DropdownMenu.Hint>
            </DropdownMenu.RadioItem>
            <DropdownMenu.RadioItem value="asc">
              Created At - Ascending
              <DropdownMenu.Hint>1 - 30</DropdownMenu.Hint>
            </DropdownMenu.RadioItem>
            <DropdownMenu.RadioItem value="desc">
              Created At - Descending
              <DropdownMenu.Hint>30 - 1</DropdownMenu.Hint>
            </DropdownMenu.RadioItem>
          </DropdownMenu.RadioGroup>
        </DropdownMenu.Content>
      </DropdownMenu>
      <span className="txt-small text-ui-fg-muted">Sorting: {sort}</span>
    </div>
  );
}
```

### `dropdown-menu-submenu.tsx`

```tsx
import { DropdownMenu, IconButton } from "@medusajs/ui";
import { BarsArrowDown } from "@medusajs/icons";

export default function DropdownMenuSubmenu() {
  return (
    <DropdownMenu>
      <DropdownMenu.Trigger asChild>
        <IconButton>
          <BarsArrowDown />
        </IconButton>
      </DropdownMenu.Trigger>
      <DropdownMenu.Content>
        <DropdownMenu.Item>Edit</DropdownMenu.Item>
        <DropdownMenu.SubMenu>
          <DropdownMenu.SubMenuTrigger>More Actions</DropdownMenu.SubMenuTrigger>
          <DropdownMenu.SubMenuContent>
            <DropdownMenu.Item>Duplicate</DropdownMenu.Item>
            <DropdownMenu.Item>Archive</DropdownMenu.Item>
          </DropdownMenu.SubMenuContent>
        </DropdownMenu.SubMenu>
        <DropdownMenu.Item>Delete</DropdownMenu.Item>
      </DropdownMenu.Content>
    </DropdownMenu>
  );
}
```

## Atlas

Org switcher, account menu, row `...` menu, Add filter. See [views.md](views.md) and [screenshots/](screenshots/).

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
