# Tailwind CSS Cheat Sheet

> Tailwind v4 — 1 unit = 4px (0.25rem)

---

## Layout

| Class | Effect |
|-------|--------|
| `container` | Sets max-width to current breakpoint |
| `block` | `display: block` |
| `inline-block` | `display: inline-block` |
| `hidden` | `display: none` |
| `flex` | `display: flex` |
| `grid` | `display: grid` |

---

## Flexbox

| Class | Effect |
|-------|--------|
| `flex-row` | Horizontal (default) |
| `flex-col` | Vertical |
| `flex-wrap` | Allow wrapping |
| `items-start` | Align cross-axis: start |
| `items-center` | Align cross-axis: center |
| `items-end` | Align cross-axis: end |
| `justify-start` | Align main-axis: start |
| `justify-center` | Align main-axis: center |
| `justify-end` | Align main-axis: end |
| `justify-between` | Space between items |
| `gap-1/2/4/8` | Gap between flex/grid children |

---

## Grid

| Class | Effect |
|-------|--------|
| `grid-cols-1` | 1 column |
| `grid-cols-2` | 2 columns |
| `grid-cols-3` | 3 columns |
| `col-span-2` | Item spans 2 columns |
| `gap-4` | Gap between cells |

---

## Spacing (Padding & Margin)

| Class | Value |
|-------|-------|
| `p-1` | 4px |
| `p-2` | 8px |
| `p-4` | 16px |
| `p-6` | 24px |
| `p-8` | 32px |
| `px-4` | Horizontal padding 16px |
| `py-4` | Vertical padding 16px |
| `pt-4 pr-4 pb-4 pl-4` | Individual sides |
| `m-4` | Margin 16px |
| `mx-auto` | Center horizontally |
| `mt-4 mb-4` | Top/bottom margin |

---

## Sizing

| Class | Effect |
|-------|--------|
| `w-full` | 100% width |
| `w-1/2` | 50% width |
| `w-64` | 256px (16rem) |
| `w-screen` | 100vw |
| `max-w-sm` | max-width: 384px |
| `max-w-md` | max-width: 448px |
| `max-w-lg` | max-width: 512px |
| `max-w-xl` | max-width: 576px |
| `max-w-2xl` | max-width: 672px |
| `max-w-4xl` | max-width: 896px |
| `h-full` | 100% height |
| `h-screen` | 100vh |
| `min-h-screen` | min-height: 100vh |

---

## Typography

| Class | Effect |
|-------|--------|
| `text-xs` | 12px |
| `text-sm` | 14px |
| `text-base` | 16px (default) |
| `text-lg` | 18px |
| `text-xl` | 20px |
| `text-2xl` | 24px |
| `text-3xl` | 30px |
| `text-4xl` | 36px |
| `font-normal` | font-weight: 400 |
| `font-medium` | font-weight: 500 |
| `font-semibold` | font-weight: 600 |
| `font-bold` | font-weight: 700 |
| `text-left/center/right` | Text alignment |
| `leading-tight` | line-height: 1.25 |
| `leading-normal` | line-height: 1.5 |
| `tracking-wide` | Letter spacing |
| `uppercase/lowercase/capitalize` | Text transform |
| `truncate` | Truncate with ellipsis |

---

## Colors (Text & Background)

Replace `{color}` with: `gray`, `red`, `blue`, `green`, `yellow`, `purple`, `pink`, `indigo`, etc.
Replace `{n}` with: `50 100 200 300 400 500 600 700 800 900`

| Class | Effect |
|-------|--------|
| `text-{color}-{n}` | Text color (e.g. `text-gray-700`) |
| `bg-{color}-{n}` | Background color (e.g. `bg-blue-600`) |
| `text-white` | White text |
| `text-black` | Black text |
| `bg-white` | White background |
| `bg-transparent` | Transparent background |

---

## Borders

| Class | Effect |
|-------|--------|
| `border` | 1px solid border |
| `border-2` | 2px border |
| `border-t/r/b/l` | Single side border |
| `border-gray-300` | Border color |
| `rounded` | border-radius: 4px |
| `rounded-md` | border-radius: 6px |
| `rounded-lg` | border-radius: 8px |
| `rounded-xl` | border-radius: 12px |
| `rounded-full` | border-radius: 9999px (pill) |

---

## Shadows & Effects

| Class | Effect |
|-------|--------|
| `shadow-sm` | Small shadow |
| `shadow` | Default shadow |
| `shadow-md` | Medium shadow |
| `shadow-lg` | Large shadow |
| `opacity-50` | 50% opacity |
| `opacity-75` | 75% opacity |

---

## Position

| Class | Effect |
|-------|--------|
| `relative` | position: relative |
| `absolute` | position: absolute |
| `fixed` | position: fixed |
| `sticky` | position: sticky |
| `top-0 right-0 bottom-0 left-0` | Position offsets |
| `inset-0` | All sides: 0 |
| `z-10 z-20 z-50` | z-index |

---

## Interactivity & States

| Class | Effect |
|-------|--------|
| `cursor-pointer` | Pointer cursor on hover |
| `cursor-not-allowed` | Disabled cursor |
| `hover:bg-blue-700` | Background on hover |
| `hover:text-white` | Text color on hover |
| `focus:outline-none` | Remove focus outline |
| `focus:ring-2` | Focus ring |
| `disabled:opacity-50` | Style when disabled |
| `transition` | Enable transitions |
| `duration-200` | Transition: 200ms |

---

## Responsive Prefixes

Mobile-first — prefix applies at that breakpoint **and up**.

| Prefix | Breakpoint |
|--------|-----------|
| (none) | All sizes |
| `sm:` | 640px+ |
| `md:` | 768px+ |
| `lg:` | 1024px+ |
| `xl:` | 1280px+ |
| `2xl:` | 1536px+ |

Example: `class="flex-col md:flex-row"` → vertical on mobile, horizontal on tablet+

---

## Dark Mode

| Class | Effect |
|-------|--------|
| `dark:bg-gray-900` | Background in dark mode |
| `dark:text-white` | Text in dark mode |

---

## Common Patterns

### Centered page content
```html
<div class="max-w-2xl mx-auto px-4 py-8">...</div>
```

### Card
```html
<div class="bg-white rounded-lg shadow-md p-6">...</div>
```

### Button
```html
<button class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-4 py-2 rounded transition">
  Click me
</button>
```

### Input field
```html
<input class="w-full border border-gray-300 rounded px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" />
```

### Navbar
```html
<nav class="flex items-center justify-between px-8 py-4 bg-gray-900 text-white">...</nav>
```

---

Sources:
- [Tailwind CSS Cheat Sheet — Skillademia](https://www.skillademia.com/cheat-sheets/tailwind-css)
- [Tailwind CSS Cheat Sheet — NerdCave](https://nerdcave.com/tailwind-cheat-sheet)
- [Tailwind CSS Cheat Sheet — Kombai](https://kombai.com/tailwind/cheat-sheet)
- [Tailwind Official Docs](https://tailwindcss.com/docs/styling-with-utility-classes)
