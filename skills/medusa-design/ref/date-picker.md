# DatePicker

Docs: https://docs.medusajs.com/ui/components/date-picker
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A field that opens a calendar to pick a date.

## When to open this file

Open this file when the view needs `DatePicker`, or a related control from the index row for `date-picker`.

## Usage

```tsx
<DatePicker />
```

## Kit props (`@medusajs/ui` 4.2.1)

- `size`: `base` | `small`

## Official examples

### `date-picker-demo.tsx`

```tsx
import { DatePicker } from "@medusajs/ui";

export default function DatePickerDemo() {
  return (
    <div className="w-[250px]">
      <DatePicker />
    </div>
  );
}
```

### `date-picker-business-hours.tsx`

```tsx
"use client";

import { DatePicker } from "@medusajs/ui";

export default function DatePickerBusinessHours() {
  return (
    <div className="w-[300px]">
      <DatePicker
        granularity="hour"
        defaultValue={new Date()}
        aria-label="Select date and hour for business scheduling"
        isDateUnavailable={(date) => {
          // Disable weekends and holidays
          const day = date.getDay();
          const isWeekend = day === 0 || day === 6;

          // Example: Disable specific holiday (Christmas)
          const isChristmas = date.getMonth() === 11 && date.getDate() === 25;

          return isWeekend || isChristmas;
        }}
      />
    </div>
  );
}
```

### `date-picker-controlled.tsx`

```tsx
"use client";

import { DatePicker } from "@medusajs/ui";
import { useState } from "react";

export default function DatePickerControlled() {
  const [date, setDate] = useState<Date | null>(new Date());

  return (
    <div className="space-y-4 w-[300px]">
      <DatePicker value={date} onChange={setDate} aria-label="Select a date" />
      <div className="text-ui-fg-subtle text-ui-body-small">
        Selected date: {date ? date.toLocaleDateString() : "None"}
      </div>
    </div>
  );
}
```

### `date-picker-disabled-dates.tsx`

```tsx
"use client";

import { DatePicker } from "@medusajs/ui";

export default function DatePickerDisabledDates() {
  // Disable weekends (Saturday and Sunday)
  const isWeekend = (date: Date) => {
    const day = date.getDay();
    return day === 0 || day === 6; // Sunday = 0, Saturday = 6
  };

  return (
    <div className="w-[300px]">
      <DatePicker isDateUnavailable={isWeekend} aria-label="Select a weekday (weekends disabled)" />
    </div>
  );
}
```

### `date-picker-form.tsx`

```tsx
"use client";

import { DatePicker, Button, Label } from "@medusajs/ui";
import { useState } from "react";

export default function DatePickerForm() {
  const [eventDate, setEventDate] = useState<Date | null>(null);
  const [reminderDate, setReminderDate] = useState<Date | null>(null);
  const [submitted, setSubmitted] = useState(false);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    setSubmitted(true);
    // Here you would typically send data to your API
    setTimeout(() => setSubmitted(false), 5000);
  };

  const isFormValid = eventDate && reminderDate;

  return (
    <div className="p-6 max-w-md border border-ui-border-base rounded-lg bg-ui-bg-base">
      <form onSubmit={handleSubmit} className="space-y-6">
        <div className="space-y-2">
          <h3 className="text-ui-fg-base font-medium">Schedule Event</h3>
          <p className="text-ui-fg-subtle text-ui-body-small">
            Set up your event and reminder dates
          </p>
        </div>

        <div className="space-y-4">
          <div className="space-y-2">
            <Label htmlFor="event-date">Event Date & Time</Label>
            <DatePicker
              id="event-date"
              value={eventDate}
              onChange={setEventDate}
              minValue={new Date()}
              aria-label="Select event date and time"
            />
          </div>

          <div className="space-y-2">
            <Label htmlFor="reminder-date">Reminder Date</Label>
            <DatePicker
              id="reminder-date"
              value={reminderDate}
              onChange={setReminderDate}
              minValue={new Date()}
              maxValue={eventDate || undefined}
              aria-label="Select reminder date"
            />
          </div>
        </div>

        <Button type="submit" disabled={!isFormValid || submitted} className="w-full">
          {submitted ? "Event Scheduled!" : "Schedule Event"}
        </Button>
      </form>

      {submitted && (
        <div className="mt-4 text-ui-fg-subtle text-ui-body-small">
          Event scheduled for {eventDate?.toLocaleString()} with a reminder on{" "}
          {reminderDate?.toLocaleDateString()}.
        </div>
      )}
    </div>
  );
}
```

### `date-picker-granularity.tsx`

```tsx
"use client";

import { DatePicker } from "@medusajs/ui";

export default function DatePickerGranularity() {
  const defaultDate = new Date();

  return (
    <div className="space-y-6 max-w-md">
      <div className="space-y-2">
        <div className="text-ui-fg-base text-ui-body-small font-medium">Date Only</div>
        <DatePicker granularity="day" defaultValue={defaultDate} aria-label="Select day only" />
      </div>

      <div className="space-y-2">
        <div className="text-ui-fg-base text-ui-body-small font-medium">
          Date and Time with Hour Precision
        </div>
        <DatePicker
          granularity="hour"
          defaultValue={defaultDate}
          aria-label="Select date and hour"
        />
      </div>

      <div className="space-y-2">
        <div className="text-ui-fg-base text-ui-body-small font-medium">
          Date and Time with Minute Precision
        </div>
        <DatePicker
          granularity="minute"
          defaultValue={defaultDate}
          aria-label="Select date and time with minutes"
        />
      </div>

      <div className="space-y-2">
        <div className="text-ui-fg-base text-ui-body-small font-medium">
          Date and Time with Second Precision
        </div>
        <DatePicker
          granularity="second"
          defaultValue={defaultDate}
          aria-label="Select date and time with seconds"
        />
      </div>
    </div>
  );
}
```

### `date-picker-min-max.tsx`

```tsx
"use client";

import { DatePicker } from "@medusajs/ui";

export default function DatePickerMinMax() {
  const today = new Date();
  const maxDate = new Date();
  maxDate.setDate(today.getDate() + 30); // 30 days from today

  return (
    <div className="w-[300px]">
      <DatePicker
        minValue={today}
        maxValue={maxDate}
        aria-label="Select a date within the next 30 days"
      />
    </div>
  );
}
```

## Atlas

Kit Inputs.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
