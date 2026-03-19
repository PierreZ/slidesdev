---
theme: ./
title: Theme Demo
---

# My Talk Title

A subtitle or description

Author Name

---

# Regular Slide

This is a regular slide with **bold accent text** and normal content.

- Bullet point one
- **Important** bullet point
- Another point

---
layout: two-cols
---

# Two Columns

Left column content here.

- Item A
- Item B

::right::

Right column content here.

```rust
fn main() {
    println!("Hello!");
}
```

---
layout: end
---

# Thank you!

[pierrezemb.fr](https://pierrezemb.fr) · [@PierreZ](https://x.com/PierreZ)

<!--
To add a footer bar, create a global-bottom.vue in your presentation:

```vue
<script setup>
import { useNav } from '@slidev/client'
const { currentSlideNo } = useNav()
</script>

<template>
  <div class="footer-bar">
    <span>#YourHashtag</span>
    <span>{{ currentSlideNo }}</span>
  </div>
</template>

<style scoped>
.footer-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 40px;
  background: var(--theme-accent);
  color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 1.5rem;
  font-size: 0.85em;
}
</style>
```
-->
