# Calendar

Docs: https://docs.medusajs.com/ui/components/calendar
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A calendar for picking dates.

## When to open this file

Open this file when the view needs `Calendar`, or a related control from the index row for `calendar`.

## Usage

```tsx
<Calendar />
```

## Kit props (`@medusajs/ui` 4.2.1)

The currently selected date.

## Official examples

### `calendar-demo.tsx`

```tsx
import { Calendar } from "@medusajs/ui";
import * as React from "react";

export default function CalendarDemo() {
  const [date, setDate] = React.useState<Date | null>();

  return <Calendar value={date} onChange={setDate} />;
}
```

### `calendar-controlled.tsx`

```tsx
import { Calendar } from "@medusajs/ui";
import { useState } from "react";

export default function CalendarControlled() {
  const [date, setDate] = useState<Date | null>(null);
  return (
    <div className="flex flex-col gap-2">
      <Calendar value={date} onChange={setDate} />
      <span className="txt-small text-ui-fg-muted">Selected: {date?.toDateString() ?? "None"}</span>
    </div>
  );
}
```

### `calendar-min-max.tsx`

```tsx
import { Calendar } from "@medusajs/ui";

export default function CalendarMinMax() {
  const min = new Date();
  const max = new Date();
  max.setDate(max.getDate() + 10);
  return (
    <div className="flex flex-col gap-2">
      <span className="txt-small text-ui-fg-muted">
        Selectable dates: {min.toDateString()} to {max.toDateString()}
      </span>
      <Calendar minValue={min} maxValue={max} />
    </div>
  );
}
```

### `calendar-unavailable.tsx`

```tsx
import { Calendar } from "@medusajs/ui";

function isUnavailable(date: Date) {
  // Disable all Sundays
  return date.getDay() === 0;
}

export default function CalendarUnavailable() {
  return (
    <div className="flex flex-col gap-2">
      <span className="txt-small text-ui-fg-muted">All Sundays are unavailable for selection.</span>
      <Calendar isDateUnavailable={isUnavailable} />
    </div>
  );
}
```

## Atlas

Not used in locked CRUD previews. Use DatePicker for a field.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
