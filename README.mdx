# CVA (Class Variance Authority)

This MDX file explains **CVA** in simple language with clear examples.

---

## 🧩 What is CVA?

CVA (Class Variance Authority) is a tiny utility that helps you:

- create **reusable** Tailwind component classes
- manage **variants** (size, color, type)
- avoid long messy `className` strings
- keep your UI **consistent**

CVA simply returns a **string of classes** based on the variants you provide.

> Think of it like: “Give me the right classes depending on the options.”

---

## 🎯 Why CVA?

Without CVA, your components may look like this:

```jsx
<button
  className={`px-4 py-2 rounded text-white
    ${variant === "primary" ? "bg-blue-600" : "bg-gray-400"}
    ${size === "sm" ? "text-sm" : "text-base"}
  `}
>
  Click
</button>
```

This is hard to maintain.

With CVA, all logic is in one place.

---

## ✅ Basic Example

```ts
import { cva } from "class-variance-authority";

const button = cva(
  "px-4 py-2 rounded font-medium transition", // base
  {
    variants: {
      variant: {
        primary: "bg-blue-600 text-white",
        secondary: "bg-gray-200 text-black",
      },
      size: {
        sm: "text-sm",
        md: "text-base",
        lg: "text-lg",
      },
    },
    defaultVariants: {
        variant: "primary",
        size: "md"
    }
  }
);
```

Usage:

```jsx
<button className={button({ variant: "secondary", size: "sm" })}>
  Click
</button>
```

---

## 🔥 What Are Compound Variants?

Compound variants let you apply **extra styles** only when **multiple conditions match together**.

> “If variant = X AND size = Y → apply this class.”

### Example

```ts
const button = cva(
  "px-4 py-2 rounded",
  {
    variants: {
      variant: {
        primary: "bg-blue-600 text-white",
        secondary: "bg-gray-200 text-black",
      },
      size: {
        sm: "text-sm",
        md: "text-base",
      },
    },

    compoundVariants: [
      {
        variant: "primary",
        size: "sm",
        class: "shadow-md", // applies only for this combo
      },
      {
        variant: "secondary",
        size: "md",
        class: "uppercase",
      },
    ],
  }
);
```

Usage:

```jsx
<button className={button({ variant: "primary", size: "sm" })}>
  Primary Small Button
</button>
```

This includes:
- base classes
- primary classes
- sm classes
- `shadow-md` (because the combo matches)

---

## 🚀 Why CVA Is Useful

- Removes messy conditional class logic
- Gives **type safety** (wrong variant values cause TypeScript errors)
- Easy to reuse across components
- Cleaner + predictable code
- Works great with design systems and shared UI libraries

---

## ⚠️ Important Things to Know

### 1. CVA does **not** merge Tailwind classes

You should wrap output with `twMerge`:

```ts
import { clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs) {
  return twMerge(clsx(inputs));
}
```

Then use:

```jsx
<button className={cn(button({ size: "sm" }))}>
  Click
</button>
```

### 2. Avoid too many compoundVariants
It becomes hard to maintain.

### 3. Limit variants
Keep things simple: 3–4 variants per component.

### 4. CVA works with any framework
React, Vue, Svelte, Solid — it’s just a function.

---

## 🔧 What Are `clsx` and `twMerge`?

### ✅ `clsx`
`clsx` is a small utility that helps you **conditionally combine class names**.

Example:
```ts
clsx("btn", isActive && "btn-active", false && "hidden");
```
This returns:
```
"btn btn-active"
```
It removes:
- false values
- null
- undefined
- empty strings

So it helps you avoid writing messy conditionals.

---

### ✅ `twMerge`
`twMerge` takes a string of Tailwind classes and **removes duplicates or conflicting classes**.

Example:
```ts
twMerge("p-2 p-4");
```
Tailwind will apply the last one, but ordering can get messy.  
`twMerge` fixes it by returning:
```
"p-4"
```

Example with conflicts:
```ts
twMerge("text-red-500 text-blue-500"); // → "text-blue-500"
```

When used with `clsx`, we build a helper:

```ts
export function cn(...inputs) {
  return twMerge(clsx(inputs));
}
```

Now `cn()`:
- joins classes (`clsx`)
- removes duplicates (`twMerge`)

This pairs perfectly with CVA.

---

## 🧩 Summary

- CVA = class generator for Tailwind.
- Helps you manage variants cleanly.
- Compound variants handle multi-condition styling.
- Use `twMerge` to remove conflicting classes.
- Great for component libraries and design systems.

---
