# Everything About `VariantProps` (CVA)

`VariantProps` is a TypeScript utility that comes with **class-variance-authority (CVA)**.  
It reads the `variants` you define inside a `cva()` function and generates a TypeScript type for them.

---

## ✅ What `VariantProps` Returns

`VariantProps<typeof yourCvaFn>` returns **one object type** where:

- each key is a **variant name**  
- each value is a **union** of allowed options  
- all keys are **optional**  

Example:

```ts
type ButtonVariants = {
  size?: "sm" | "md" | "lg";
  tone?: "primary" | "neutral";
};
```

---

## 🧩 Example with CVA

```ts
import { cva } from "class-variance-authority";
import type { VariantProps } from "class-variance-authority";

const button = cva("base-styles", {
  variants: {
    size: {
      sm: "text-sm px-2 py-1",
      md: "text-base px-3 py-2",
      lg: "text-lg px-4 py-3",
    },
    tone: {
      primary: "bg-blue-600 text-white",
      neutral: "bg-gray-200 text-black",
    },
  },
  defaultVariants: {
    size: "md",
    tone: "primary",
  },
});

type ButtonProps = VariantProps<typeof button>;
```

What TypeScript infers:

```ts
// Equivalent:
type ButtonProps = {
  size?: "sm" | "md" | "lg";
  tone?: "primary" | "neutral";
};
```

---

## 🎯 Why `VariantProps` Is Useful

- Auto-generates correct prop types  
- Keeps your component props in sync with CVA variants  
- Prevents invalid values (e.g., `size="huge"` gets a TS error)  
- No need to manually write variant types  

---

## 🚀 Using It in a React Component

```tsx
import { cn } from "@/lib/utils"; // clsx + twMerge

export function Button({ size, tone, className, ...props }: ButtonProps) {
  return (
    <button
      className={cn(button({ size, tone }), className)}
      {...props}
    />
  );
}
```

Now TypeScript enforces:

- `size` can only be: `"sm" | "md" | "lg"`  
- `tone` can only be: `"primary" | "neutral"`  

---

## 🔍 Extra Notes

- `defaultVariants` **do not** make props required — they stay optional.
- `VariantProps` does **not** include any `defaultVariants` logic; it's only a type extractor.
- Compound variants do not affect types — they use the same variant keys.
- It's purely a **compile-time type**, not a runtime function.

