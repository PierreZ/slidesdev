<template>
  <footer class="footer-bar">
    <span>#finistdevs #finistAI #yncrea</span>
    <span>{{ $nav.currentPage }}</span>
  </footer>
</template>

<style scoped>
.footer-bar {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 30px;
  background: var(--theme-accent);
  color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 1.5rem;
  font-size: 0.85em;
  font-weight: 600;
  z-index: 100;
}
</style>
