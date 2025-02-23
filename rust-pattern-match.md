---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Rust 的流程控制与模式匹配
info: |
  Rust 流程控制与模式匹配深度解析
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
color: stone-light
---

# Rust 流程控制与模式匹配

安全性与表达力并重的系统级编程

<br>

2025.02.23

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
color: stone-light
---

# Why Rust Control Flow?

Rust 控制流核心设计理念

- **表达式优先**：所有控制结构返回可计算值
- **模式匹配集成**：编译时穷尽性检查
- **内存安全保证**：所有权系统与流程控制结合

```rust
// 表达式示例
let status = if user.is_admin() {
    AccessLevel::Admin
} else {
    AccessLevel::User // 必须返回相同类型
};
```

---
layout: two-cols
layoutClass: gap-16
---

# Table of contents

You can use the `Toc` component to generate a table of contents for your slides:

```html
<Toc minDepth="1" maxDepth="1" />
```

The title will be inferred from your slide content, or you can override it with `title` and `level` in your frontmatter.

::right::

<Toc text-sm minDepth="1" maxDepth="1" />

---
layout: image-right
image: https://cover.sli.dev
---

# Code

Use code snippets and get the highlighting directly, and even types hover!

```ts {all|5|7|7-8|10|all} twoslash
// TwoSlash enables TypeScript hover information
// and errors in markdown code blocks
// More at https://shiki.style/packages/twoslash

import { computed, ref } from 'vue'

const count = ref(0)
const doubled = computed(() => count.value * 2)

doubled.value = 2
```

<arrow v-click="[4, 5]" x1="350" y1="310" x2="195" y2="334" color="#953" width="2" arrowSize="1" />

<!-- This allow you to embed external code blocks -->
<<< @/snippets/external.ts#snippet

<!-- Footer -->

[Learn more](https://sli.dev/features/line-highlighting)

<!-- Inline style -->
<style>
.footnotes-sep {
  @apply mt-5 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---
transition: slide-up
level: 2
---

# Navigation

Hover on the bottom-left corner to see the navigation's controls panel, [learn more](https://sli.dev/guide/ui#navigation-bar)

## Keyboard Shortcuts

|                                                     |                             |
| --------------------------------------------------- | --------------------------- |
| <kbd>right</kbd> / <kbd>space</kbd>                 | next animation or slide     |
| <kbd>left</kbd>  / <kbd>shift</kbd><kbd>space</kbd> | previous animation or slide |
| <kbd>up</kbd>                                       | previous slide              |
| <kbd>down</kbd>                                     | next slide                  |

<!-- https://sli.dev/guide/animations.html#click-animation -->
<img
  v-click
  class="absolute -bottom-9 -left-7 w-80 opacity-50"
  src="https://sli.dev/assets/arrow-bottom-left.svg"
  alt=""
/>
<p v-after class="absolute bottom-23 left-45 opacity-30 transform -rotate-10">Here!</p>

---
layout: two-cols
layoutClass: gap-16
---

# 基础流程控制

```rust
let x = 5;
let size = if x < 0 { "small" } 
    else if x < 10 { "medium" }
    else { "large" };
```

```rust
// 带返回值的loop
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};
```

::right::

## 模式匹配基础
```rust
match value {
    1 => println!("One"),
    2 | 3 => println!("Two or Three"),
    4..=10 => println!("4-10"),
    _ => println!("Other"),
}
```

---
transition: slide-up
---

# 深度模式解构

解构复杂数据结构

```rust
struct Point { x: i32, y: i32 }

let point = Point { x: 10, y: 5 };

match point {
    Point { x, y: 0 } => println!("On x axis at {}", x),
    Point { x: 0..=5, y } => println!("Left zone y={}", y),
    Point { x, y } if x == y => println!("On diagonal"),
    _ => println!("Somewhere else"),
}
```

**高级技巧**：
- `@` 绑定：`Some(x @ 1..=10)`
- 嵌套模式：`&[a, b, c]`
- 通配符：`_` 和 `..`

---
layout: default
---

# 实战模式

**Option/Result 处理范式**

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
    match b {
        0.0 => None,
        _ => Some(a / b),
    }
}

let result = divide(10.0, 2.0);
match result {
    Some(x) if x > 5.0 => println!("Large result: {}", x),
    Some(x) => println!("Result: {}", x),
    None => println!("Cannot divide by zero"),
}
```

---
class: px-20
---

# 最佳实践与对比

| 场景                | Rust 方案          | Go 方案           | C++ 方案         |
|---------------------|--------------------|-------------------|------------------|
| 错误处理            | Result + match     | 多返回值          | 异常机制         |
| 空值处理            | Option             | nil               | nullptr          |
| 类型切换            | 模式匹配           | type switch       | dynamic_cast     |
| 循环控制            | 迭代器 + for       | range + for       | 传统 for/while   |

**性能提示**：
- 优先使用迭代器而非索引访问
- match 在编译时优化为跳转表
- 避免过度嵌套的模式匹配

---
layout: center
class: text-center
---

# 进阶学习资源

[The Rust Programming Language](https://doc.rust-lang.org/book/)  
[Rust 模式匹配指南](https://rust-lang.github.io/rust-patterns/)  
[编译器错误解读实践](https://blog.rust-lang.org/2016/08/10/Shape-of-errors-to-come.html)

<button class="mt-4">开始编码挑战 →</button>
