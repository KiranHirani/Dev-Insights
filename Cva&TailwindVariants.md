# CVA vs Tailwind Variants — Deep Dive for Component Libraries (shadcn)

This guide explores in depth how **class-variance-authority (CVA)** compares to **Tailwind Variants (TV)** — especially for teams building **component libraries using shadcn + Tailwind CSS**.

---

## 🧩 What They Solve

Both CVA and Tailwind Variants exist to handle **conditional Tailwind class management** cleanly.  
Without them, your components can quickly get messy:

```tsx
<button
  className={`${primary ? 'bg-blue-500' : 'bg-gray-500'} ${
    size === 'lg' ? 'p-4' : 'p-2'
  }`}
>
  Click me
</button>
```

Both libraries let you write declarative, reusable class structures for variants and states.

---

## 🧱 CVA (class-variance-authority)

### Example

```tsx
import { cva } from "class-variance-authority";

const button = cva(
  "rounded font-medium transition-colors",
  {
    variants: {
      intent: {
        primary: "bg-blue-500 text-white hover:bg-blue-600",
        secondary: "bg-gray-100 text-gray-800 hover:bg-gray-200",
      },
      size: {
        sm: "px-2 py-1 text-sm",
        md: "px-4 py-2 text-base",
      },
    },
    defaultVariants: {
      intent: "primary",
      size: "md",
    },
    compoundVariants: [
      {
        intent: "secondary",
        size: "sm",
        class: "border border-gray-300",
      },
    ],
  }
);

// usage
<button className={button({ intent: "secondary", size: "sm" })}>
  Button
</button>;
```

### ✅ Pros

- Simple, lightweight, and stable.
- Good for defining variant combinations.
- Built-in support for **compoundVariants**.

### ⚠️ Limitations

- Cannot define **slots** (like wrapper, icon, label parts).
- Cannot **merge multiple CVA definitions** directly.
- Slightly verbose compared to Tailwind Variants.
- TypeScript autocomplete is limited (you manually type variant keys).

---

## 🌈 Tailwind Variants (tv)

`tailwind-variants` is a newer, modernized replacement used in latest **shadcn/ui**.

### Example

```tsx
import { tv } from "tailwind-variants";

const button = tv({
  base: "inline-flex items-center rounded font-medium transition-colors",
  variants: {
    intent: {
      primary: "bg-blue-500 text-white hover:bg-blue-600",
      secondary: "bg-gray-100 text-gray-800 hover:bg-gray-200",
    },
    size: {
      sm: "px-2 py-1 text-sm",
      md: "px-4 py-2 text-base",
    },
  },
  compoundVariants: [
    {
      intent: "secondary",
      size: "sm",
      class: "border border-gray-300",
    },
  ],
  defaultVariants: {
    intent: "primary",
    size: "md",
  },
});
```

Usage:

```tsx
<button className={button({ intent: "secondary", size: "sm" })}>
  Click
</button>
```

### ✅ Advantages (in depth)

#### 1. Cleaner Syntax & TypeScript Support
TV automatically infers types for variants, so VSCode autocompletes and validates your props.

```tsx
button({ intent: "prim" }); // ❌ Error: only "primary" | "secondary" allowed
```

#### 2. Built-in Class Merging
When user passes extra `className`, TV handles merge with proper **Tailwind CSS precedence** (later classes override earlier ones).

```tsx
<button className={button({ class: "bg-red-500" })}>...</button>
```
→ final class will use **red background** because it’s last in the order.

#### 3. Composability (Merging Definitions)
You can merge multiple variant systems easily.

```tsx
const base = tv({ base: "text-sm font-medium" });
const alert = tv({ extend: base, base: "rounded p-4", variants: {
  intent: { info: "bg-blue-100", warning: "bg-yellow-100" }
}});
```

This means you can create **primitive building blocks** and extend them per component.

#### 4. Slots Support 💎

TV supports **slots**, which let you define styles for internal parts of a component (icon, wrapper, label).

```tsx
const card = tv({
  slots: {
    root: "rounded-lg shadow p-4 bg-white",
    header: "text-lg font-semibold mb-2",
    body: "text-gray-700",
  },
  variants: {
    intent: {
      info: { root: "border-blue-300", header: "text-blue-700" },
    },
  },
  defaultVariants: { intent: "info" },
});

const { root, header, body } = card();

<div className={root()}>
  <h2 className={header()}>Card Title</h2>
  <p className={body()}>Body content</p>
</div>;
```

This is **impossible in CVA** because CVA only returns one class string.

#### 5. Better Type Safety
Each variant and slot is strongly typed; errors surface immediately in the editor.

#### 6. Future Proofing
All new **shadcn/ui** components now use Tailwind Variants — this will be the ongoing standard.

---

## ⚖️ Merging Variants — How It Works

### In Tailwind Variants

```tsx
const base = tv({
  variants: { size: { sm: "p-2", lg: "p-6" } },
  defaultVariants: { size: "sm" },
});

const extended = tv({
  extend: base,
  variants: { color: { primary: "bg-blue-500", danger: "bg-red-500" } },
});
```

When you call `extended({ size: "lg", color: "primary" })`,  
both variant sets merge correctly, and Tailwind CSS will resolve the latest one.

This level of merging (with type preservation) is **not possible with CVA** directly.

### In CVA
You’d have to manually compose classNames using `clsx` or `twMerge`:

```tsx
const merged = clsx(button({ size: "sm" }), anotherVariant({ intent: "warning" }));
```

You lose type inference and variant awareness.

---

## 🎛️ CSS Precedence Rules

Both CVA and TV output **plain Tailwind classes**, so CSS specificity rules apply as usual.

However, **TV merges classes** in a predictable order:

1. `base` classes (lowest priority)
2. `variant` classes
3. `compoundVariants`
4. `user-passed class` (highest priority)

So user overrides **always win**.

Example:

```tsx
<button className={button({ intent: "primary", class: "bg-red-500" })}>
  Red wins
</button>
```

→ The background will be red, not blue.

---

## ⚗️ Compound Variants — Comparison

Both support compound variants, but **TV** allows nested or slot-based compounds.

### CVA

```tsx
compoundVariants: [
  {
    intent: "secondary",
    size: "sm",
    class: "border border-gray-300",
  },
]
```

### TV

```tsx
compoundVariants: [
  {
    intent: "secondary",
    size: "sm",
    class: { root: "border border-gray-300", icon: "text-gray-500" },
  },
]
```

→ CVA can only attach compound classes to one element.  
→ TV supports slot-aware compounds — super useful for component libraries.

---

## 🎨 CSS Class Override (User Input)

If a user passes `class` manually, both CVA and TV merge them.

But in **CVA**, you must wrap it yourself:

```tsx
clsx(button({ intent: "primary" }), className);
```

In **TV**, it’s built-in:

```tsx
button({ intent: "primary", class: className });
```

The `class` key is a first-class citizen in TV.

---

## 🧩 TypeScript Developer Experience

| Feature | CVA | Tailwind Variants |
|----------|------|------------------|
| Variant autocomplete | ❌ Manual | ✅ Auto inferred |
| Slot typing | ❌ Not supported | ✅ Supported |
| Compound variant typing | Basic | Advanced |
| Merge definitions | ❌ Manual | ✅ extend |
| Default variants | ✅ Yes | ✅ Yes |

---

## 💡 When to Choose What

| Use Case | Recommended |
|-----------|--------------|
| Existing project with CVA setup | Stay on CVA (no breaking need to switch) |
| New project / component library | ✅ Tailwind Variants |
| Need slot-based styles (icon, label, wrapper) | ✅ Tailwind Variants |
| Want minimal dependencies | CVA |
| Want type safety, autocomplete, composability | ✅ Tailwind Variants |

---

## 🧭 Final Recommendation

If you’re building or modernizing a **component library using shadcn**,  
go with **Tailwind Variants (`tv`)**. It’s more composable, slot-aware, and type-safe — exactly what scalable UI systems need.

CVA remains fine for older setups, but TV provides a **cleaner DX**, **stronger typing**, and **better structure** for reusable, themeable components.
