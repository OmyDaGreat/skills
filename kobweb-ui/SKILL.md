---
name: kobweb-ui-best-practices
description: Guidelines for idiomatic, maintainable Kobweb Compose UI. Enforce Modifier usage and shared style patterns. Use this skill when reviewing, writing, or refactoring Kobweb UI code.
license: MIT
---

# Kobweb UI Best Practices

Guidelines for idiomatic, maintainable Kobweb Compose UI code. Enforce usage of `Modifier` chains, promote shared styles, and avoid anti-patterns like direct `attrs` or inline style blocks.

**Tradeoff:** These rules optimize for idiomatic code and long-term maintainability in [Kobweb](https://github.com/varabyte/kobweb) Kotlin Compose projects. Deviate only with good reason.

## 1. Prefer `Modifier` Chains over `attrs` and Inline Styles

**Don't:**
- Use `attrs` or `style` blocks for setting layout, color, font, etc., when a `Modifier` equivalent exists.
- Mix style/logic in attribute lambdas that are better represented as composable Modifier calls.

**Do:**
- Use `Modifier` for consistently supported properties (e.g., padding, color, border, background).

**Examples:**

❌ **Anti-pattern:**
```kotlin
Button(attrs = {
  style {
    property("background", "blue")
    property("padding", "10px")
  }
}) { Text("Click") }
```

✅ **Pattern:**
```kotlin
Button(
  attrs = Modifier
    .background(Color.Blue)
    .padding(10.px)
    .toAttrs()
) { Text("Click") }
```

## 2. Extract Shared Modifier Patterns

**Don't:**
- Copy-paste long Modifier chains in multiple files or within one screen.

**Do:**
- Move common Modifier chains into an object (`Styles` or similar) or function for reuse.

**Examples:**

❌ **Anti-pattern:**
```kotlin
Box(
  Modifier
    .padding(16.px)
    .border(2.px, LineStyle.Solid, Color.Black)
    .background(Color.White)
)
// ... dozens of times
```

✅ **Pattern:**
```kotlin
object Styles {
  val Card = Modifier
    .padding(16.px)
    .border(2.px, LineStyle.Solid, Color.Black)
    .background(Color.White)
}

Box(Styles.Card)
```

## 3. Compose for Reuse and Clarity

**Do:**
- Wrap frequently used UI/builders in composable functions.
- Favor descriptive names (e.g., `PrimaryButton`, `InfoCard`).

**Don't:**
- Obscure intent with cryptic, unnamed Modifier chains.

---

## Usage Instructions

1. Use these guidelines when writing new code, reviewing PRs, or refactoring existing Kobweb Compose files.
2. For every code block:
    - Flag direct attr/style usage where Modifier equivalents exist.
    - Flag copy-pasted or near-identical Modifier chains and suggest extraction.
    - Recommend extracting common UI patterns to composable functions.
3. When there are violations, offer before/after suggestions.

**Success Criteria:**  
Kobweb UI code should use Modifiers idiomatically, minimize duplication, and separate reusable styles/components for clarity and maintainability.