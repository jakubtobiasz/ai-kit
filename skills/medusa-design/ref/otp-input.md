# OtpInput

Docs: https://docs.medusajs.com/ui/components/otp-input
Import from `@medusajs/ui`. Installed kit is `4.2.1`.

A one-time-password digit field.

## When to open this file

Open this file when the view needs `OtpInput`, or a related control from the index row for `otp-input`.

## Usage

```tsx
const [otp, setOtp] = useState("");

return <OtpInput value={otp} onChange={setOtp} />;
```

## Kit props (`@medusajs/ui` 4.2.1)

Whether the inputs should focus the first field on mount.

Flags:

- `disabled`

## Official examples

### `otp-input-demo.tsx`

```tsx
import { OtpInput } from "@medusajs/ui";
import { useState } from "react";

export default function OtpInputDemo() {
  const [otp, setOtp] = useState("");
  return (
    <div className="w-[250px]">
      <OtpInput value={otp} onChange={setOtp} />
    </div>
  );
}
```

### `otp-input-custom-separator.tsx`

```tsx
import { OtpInput } from "@medusajs/ui";
import { useState } from "react";

export default function OtpInputCustomSeparator() {
  const [otp, setOtp] = useState("");
  return (
    <div className="w-[250px]">
      <OtpInput value={otp} onChange={setOtp} separator="•" />
    </div>
  );
}
```

### `otp-input-error.tsx`

```tsx
import { OtpInput } from "@medusajs/ui";
import { useState } from "react";

export default function OtpInputError() {
  const [otp, setOtp] = useState("123");
  return (
    <div className="w-[250px]">
      <OtpInput value={otp} onChange={setOtp} aria-invalid={true} />
    </div>
  );
}
```

### `otp-input-group-size.tsx`

```tsx
import { OtpInput } from "@medusajs/ui";
import { useState } from "react";

export default function OtpInputGroupSize() {
  const [otp, setOtp] = useState("");
  return (
    <div className="w-[250px]">
      <OtpInput value={otp} onChange={setOtp} groupSize={2} />
    </div>
  );
}
```

### `otp-input-on-complete.tsx`

```tsx
import { OtpInput, Text } from "@medusajs/ui";
import { useState } from "react";

export default function OtpInputOnComplete() {
  const [otp, setOtp] = useState("");
  const [submitted, setSubmitted] = useState<string | null>(null);

  return (
    <div className="flex w-[250px] flex-col items-center gap-y-3">
      <OtpInput
        value={otp}
        onChange={(value) => {
          setOtp(value);
          if (submitted) {
            setSubmitted(null);
          }
        }}
        onComplete={(value) => setSubmitted(value)}
      />
      {submitted && <Text size="small">Submitted code: {submitted}</Text>}
    </div>
  );
}
```

### `otp-input-pin.tsx`

```tsx
import { OtpInput } from "@medusajs/ui";
import { useState } from "react";

export default function OtpInputPin() {
  const [pin, setPin] = useState("");
  return (
    <div className="w-[250px]">
      <OtpInput value={pin} onChange={setPin} length={4} groupSize={4} />
    </div>
  );
}
```

## Atlas

Not used in locked CRUD previews.

For a simple screen, follow the locked preview in `views.md`. Do not invent a new layout unless the human asks.
