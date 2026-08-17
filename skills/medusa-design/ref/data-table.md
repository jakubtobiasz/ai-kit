# DataTable

Docs: https://docs.medusajs.com/ui/components/data-table
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A table with search, filters, sort, and pagination.

## When to open this file

Open this file when the view needs `DataTable`, or a related control from the index row for `data-table`.

## Usage

```tsx
DataTable,
} from "@medusajs/ui"
```

```tsx
// ...
  createDataTableColumnHelper,
} from "@medusajs/ui"

const data = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
  },
  // other data...
]

const columnHelper = createDataTableColumnHelper<typeof data[0]>()

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
  }),
]
```

```tsx
// ...
  useDataTable,
} from "@medusajs/ui"
```

## Kit props (`@medusajs/ui` 4.2.1)

The props of the `DataTable` component.

## Official examples

### `data-table-demo.tsx`

```tsx
import { createDataTableColumnHelper, useDataTable, DataTable, Heading } from "@medusajs/ui";

const products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
];

export default function ProductTable() {
  const table = useDataTable({
    columns,
    data: products,
    getRowId: (product) => product.id,
    rowCount: products.length,
    isLoading: false,
  });

  return (
    <DataTable instance={table}>
      <DataTable.Toolbar className="flex flex-col items-start justify-between gap-2 md:flex-row md:items-center">
        <Heading>Products</Heading>
      </DataTable.Toolbar>
      <DataTable.Table />
    </DataTable>
  );
}
```

### `data-table-commands.tsx`

```tsx
import {
  DataTable,
  DataTableRowSelectionState,
  Heading,
  createDataTableColumnHelper,
  createDataTableCommandHelper,
  useDataTable,
} from "@medusajs/ui";
import { useState } from "react";

let products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  // Commands requires a select column.
  columnHelper.select(),
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
];

const commandHelper = createDataTableCommandHelper();

const useCommands = () => {
  return [
    commandHelper.command({
      label: "Delete",
      shortcut: "D",
      action: async (selection) => {
        const productsToDeleteIds = Object.keys(selection);

        alert(`You deleted product(s) with IDs: ${productsToDeleteIds.join()}`);
      },
    }),
  ];
};

export default function ProductTable() {
  const [rowSelection, setRowSelection] = useState<DataTableRowSelectionState>({});

  const commands = useCommands();

  const instance = useDataTable({
    data: products,
    columns,
    getRowId: (product) => product.id,
    rowCount: products.length,
    isLoading: false,
    commands,
    rowSelection: {
      state: rowSelection,
      onRowSelectionChange: setRowSelection,
    },
  });

  return (
    <DataTable instance={instance}>
      <DataTable.Toolbar className="flex justify-between items-center">
        <Heading>Products</Heading>
      </DataTable.Toolbar>
      <DataTable.Table />
      {/** This component will the command bar when the user has selected at least one row. **/}
      <DataTable.CommandBar selectedLabel={(count) => `${count} selected`} />
    </DataTable>
  );
}
```

### `data-table-custom-cell.tsx`

```tsx
import { createDataTableColumnHelper, useDataTable, DataTable, Heading, Badge } from "@medusajs/ui";

const products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
    is_active: true,
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
    is_active: false,
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
  columnHelper.accessor("is_active", {
    header: "Status",
    cell: ({ getValue }) => {
      const isActive = getValue();
      return (
        <Badge color={isActive ? "green" : "grey"} size="xsmall">
          {isActive ? "Active" : "Inactive"}
        </Badge>
      );
    },
  }),
];

export default function ProductTable() {
  const table = useDataTable({
    columns,
    data: products,
    getRowId: (product) => product.id,
    rowCount: products.length,
    isLoading: false,
  });

  return (
    <DataTable instance={table}>
      <DataTable.Toolbar className="flex flex-col items-start justify-between gap-2 md:flex-row md:items-center">
        <Heading>Products</Heading>
      </DataTable.Toolbar>
      <DataTable.Table />
    </DataTable>
  );
}
```

### `data-table-filters-date.tsx`

```tsx
import {
  DataTable,
  DataTableFilteringState,
  Heading,
  createDataTableColumnHelper,
  createDataTableFilterHelper,
  useDataTable,
} from "@medusajs/ui";
import { useMemo, useState } from "react";

const products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
    created_at: new Date(),
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
    created_at: new Date("2026-01-01"),
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
  columnHelper.accessor("created_at", {
    header: "Created At",
    cell: ({ getValue }) => {
      return getValue().toLocaleString();
    },
  }),
];

const filterHelper = createDataTableFilterHelper<(typeof products)[0]>();

const filters = [
  filterHelper.accessor("created_at", {
    type: "date",
    label: "Created At",
    format: "date",
    formatDateValue: (date) => date.toLocaleString(),
    rangeOptionStartLabel: "From",
    rangeOptionEndLabel: "To",
    rangeOptionLabel: "Between",
    options: [
      {
        label: "Today",
        value: {
          $gte: new Date(new Date().setHours(0, 0, 0, 0)).toString(),
          $lte: new Date(new Date().setHours(23, 59, 59, 999)).toString(),
        },
      },
      {
        label: "Yesterday",
        value: {
          $gte: new Date(new Date().setHours(0, 0, 0, 0) - 24 * 60 * 60 * 1000).toString(),
          $lte: new Date(new Date().setHours(0, 0, 0, 0)).toString(),
        },
      },
      {
        label: "Last Week",
        value: {
          $gte: new Date(new Date().setHours(0, 0, 0, 0) - 7 * 24 * 60 * 60 * 1000).toString(),
          $lte: new Date(new Date().setHours(0, 0, 0, 0)).toString(),
        },
      },
    ],
  }),
];

export default function ProductTable() {
  const [filtering, setFiltering] = useState<DataTableFilteringState>({});

  const shownProducts = useMemo(() => {
    return products.filter((product) => {
      return Object.entries(filtering).every(([key, value]) => {
        if (!value) {
          return true;
        }
        if (typeof value === "string") {
          // @ts-ignore
          return product[key].toString().toLowerCase().includes(value.toString().toLowerCase());
        }
        if (Array.isArray(value)) {
          // @ts-ignore
          return value.includes(product[key].toLowerCase());
        }
        if (typeof value === "object") {
          // @ts-ignore
          const date = new Date(product[key]);
          let matching = false;
          if ("$gte" in value && value.$gte) {
            matching = date >= new Date(value.$gte as number);
          }
          if ("$lte" in value && value.$lte) {
            matching = date <= new Date(value.$lte as number);
          }
          if ("$lt" in value && value.$lt) {
            matching = date < new Date(value.$lt as number);
          }
          if ("$gt" in value && value.$gt) {
            matching = date > new Date(value.$gt as number);
          }
          return matching;
        }
      });
    });
  }, [filtering]);

  const table = useDataTable({
    data: shownProducts,
    columns,
    getRowId: (product) => product.id,
    rowCount: products.length,
    isLoading: false,
    filtering: {
      state: filtering,
      onFilteringChange: setFiltering,
    },
    filters,
  });

  return (
    <DataTable instance={table}>
      <DataTable.Toolbar className="flex justify-between items-center">
        <Heading>Products</Heading>
        {/** This component will render a menu that allows the user to choose which filters to apply to the table data. **/}
        <DataTable.FilterMenu tooltip="Filter" />
      </DataTable.Toolbar>
      <DataTable.Table />
    </DataTable>
  );
}
```

### `data-table-filters-initial.tsx`

```tsx
import {
  DataTable,
  DataTableFilteringState,
  Heading,
  createDataTableColumnHelper,
  createDataTableFilterHelper,
  useDataTable,
} from "@medusajs/ui";
import { useMemo, useState } from "react";

const products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
];

const filterHelper = createDataTableFilterHelper<(typeof products)[0]>();

const filters = [
  filterHelper.accessor("title", {
    type: "select",
    label: "Title",
    options: products.map((product) => ({
      label: product.title,
      value: product.title.toLowerCase(),
    })),
  }),
];

export default function ProductTable() {
  const [filtering, setFiltering] = useState<DataTableFilteringState>({
    title: ["shirt"],
  });

  const shownProducts = useMemo(() => {
    return products.filter((product) => {
      return Object.entries(filtering).every(([key, value]) => {
        if (!value) {
          return true;
        }
        if (typeof value === "string") {
          // @ts-ignore
          return product[key].toString().toLowerCase().includes(value.toString().toLowerCase());
        }
        if (Array.isArray(value)) {
          // @ts-ignore
          return value.includes(product[key].toLowerCase());
        }
        if (typeof value === "object") {
          // @ts-ignore
          const date = new Date(product[key]);
          let matching = false;
          if ("$gte" in value && value.$gte) {
            matching = date >= new Date(value.$gte as number);
          }
          if ("$lte" in value && value.$lte) {
            matching = date <= new Date(value.$lte as number);
          }
          if ("$lt" in value && value.$lt) {
            matching = date < new Date(value.$lt as number);
          }
          if ("$gt" in value && value.$gt) {
            matching = date > new Date(value.$gt as number);
          }
          return matching;
        }
      });
    });
  }, [filtering]);

  const table = useDataTable({
    data: shownProducts,
    columns,
    getRowId: (product) => product.id,
    rowCount: products.length,
    isLoading: false,
    filtering: {
      state: filtering,
      onFilteringChange: setFiltering,
    },
    filters,
  });

  return (
    <DataTable instance={table}>
      <DataTable.Toolbar className="flex justify-between items-center">
        <Heading>Products</Heading>
        {/** This component will render a menu that allows the user to choose which filters to apply to the table data. **/}
        <DataTable.FilterMenu tooltip="Filter" />
      </DataTable.Toolbar>
      <DataTable.Table />
    </DataTable>
  );
}
```

### `data-table-filters.tsx`

```tsx
import {
  DataTable,
  DataTableFilteringState,
  Heading,
  createDataTableColumnHelper,
  createDataTableFilterHelper,
  useDataTable,
} from "@medusajs/ui";
import { useMemo, useState } from "react";

const products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
];

const filterHelper = createDataTableFilterHelper<(typeof products)[0]>();

const filters = [
  filterHelper.accessor("title", {
    type: "select",
    label: "Title",
    options: products.map((product) => ({
      label: product.title,
      value: product.title.toLowerCase(),
    })),
  }),
];

export default function ProductTable() {
  const [filtering, setFiltering] = useState<DataTableFilteringState>({});

  const shownProducts = useMemo(() => {
    return products.filter((product) => {
      return Object.entries(filtering).every(([key, value]) => {
        if (!value) {
          return true;
        }
        if (typeof value === "string") {
          // @ts-ignore
          return product[key].toString().toLowerCase().includes(value.toString().toLowerCase());
        }
        if (Array.isArray(value)) {
          // @ts-ignore
          return value.includes(product[key].toLowerCase());
        }
        if (typeof value === "object") {
          // @ts-ignore
          const date = new Date(product[key]);
          let matching = false;
          if ("$gte" in value && value.$gte) {
            matching = date >= new Date(value.$gte as number);
          }
          if ("$lte" in value && value.$lte) {
            matching = date <= new Date(value.$lte as number);
          }
          if ("$lt" in value && value.$lt) {
            matching = date < new Date(value.$lt as number);
          }
          if ("$gt" in value && value.$gt) {
            matching = date > new Date(value.$gt as number);
          }
          return matching;
        }
      });
    });
  }, [filtering]);

  const table = useDataTable({
    data: shownProducts,
    columns,
    getRowId: (product) => product.id,
    rowCount: products.length,
    isLoading: false,
    filtering: {
      state: filtering,
      onFilteringChange: setFiltering,
    },
    filters,
  });

  return (
    <DataTable instance={table}>
      <DataTable.Toolbar className="flex justify-between items-center">
        <Heading>Products</Heading>
        {/** This component will render a menu that allows the user to choose which filters to apply to the table data. **/}
        <DataTable.FilterMenu tooltip="Filter" />
      </DataTable.Toolbar>
      <DataTable.Table />
    </DataTable>
  );
}
```

### `data-table-pagination.tsx`

```tsx
import {
  DataTable,
  Heading,
  createDataTableColumnHelper,
  useDataTable,
  type DataTablePaginationState,
} from "@medusajs/ui";
import { useMemo, useState } from "react";

const products = [
  {
    id: "1",
    title: "Shirt",
    price: 10,
  },
  {
    id: "2",
    title: "Pants",
    price: 20,
  },
  {
    id: "3",
    title: "Hat",
    price: 15,
  },
  {
    id: "4",
    title: "Socks",
    price: 5,
  },
  {
    id: "5",
    title: "Shoes",
    price: 50,
  },
  {
    id: "6",
    title: "Jacket",
    price: 100,
  },
  {
    id: "7",
    title: "Scarf",
    price: 25,
  },
  {
    id: "8",
    title: "Gloves",
    price: 12,
  },
  {
    id: "9",
    title: "Belt",
    price: 18,
  },
  {
    id: "10",
    title: "Sunglasses",
    price: 30,
  },
  {
    id: "11",
    title: "Watch",
    price: 200,
  },
  {
    id: "12",
    title: "Tie",
    price: 20,
  },
  {
    id: "13",
    title: "Sweater",
    price: 40,
  },
  {
    id: "14",
    title: "Jeans",
    price: 60,
  },
  {
    id: "15",
    title: "Shorts",
    price: 25,
  },
  {
    id: "16",
    title: "Blouse",
    price: 35,
  },
  {
    id: "17",
    title: "Dress",
    price: 80,
  },
];

const columnHelper = createDataTableColumnHelper<(typeof products)[0]>();

const columns = [
  columnHelper.accessor("title", {
    header: "Title",
    enableSorting: true,
  }),
  columnHelper.accessor("price", {
    header: "Price",
    enableSorting: true,
  }),
];

const PAGE_SIZE = 10;

export default function ProductTable() {
  const [pagination, setPagination] = useState<DataTablePaginationState>({
    pageSize: PAGE_SIZE,
    pageIndex: 0,
  });

  const shownProducts = useMemo(() => {
    return products.slice(
      pagination.pageIndex * pagination.pageSize,
      (pagination.pageIndex + 1) * pagination.pageSize,
    );
  }, [pagination]);

  const table = useDataTable({
    data: shownProducts,
    columns,
    rowCount: products.length,
    getRowId: (product) => product.id,
    pagination: {
      // Pass the pagination state and updater to the table instance
      state: pagination,
      onPaginationChange: setPagination,
    },
    isLoading: false,
  });

  return (
    <DataTable instance={table}>
      <DataTable.Toolbar>
        <Heading>Products</Heading>
      </DataTable.Toolbar>
      <DataTable.Table />
      {/** This component will render the pagination controls **/}
      <DataTable.Pagination />
    </DataTable>
  );
}
```

## Atlas

Do not use DataTable.FilterBar on Atlas list. It looped under this stack. Locked list uses `Table` plus Add filter chips. See [views.md](views.md).

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
