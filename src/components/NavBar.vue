<script setup>
const weekNums = Array.from({ length: 15 }, (_, i) => i + 1)

function scrollTo(id) {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <nav class="navbar">
    <div class="navbar-brand">
      <span class="navbar-code">07-034-266</span>
      <span class="navbar-name">การสร้างธุรกิจเริ่มต้นฯ</span>
    </div>
    <div class="navbar-links">
      <button
        v-for="w in weekNums"
        :key="w"
        class="week-link"
        @click="scrollTo(`week-${w}`)"
      >
        W{{ w }}
      </button>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 2rem;
  background: var(--text);
  color: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  gap: 1rem;
  flex-wrap: wrap;
}

.navbar-brand {
  display: flex;
  flex-direction: column;
  line-height: 1.2;
}

.navbar-code {
  font-size: 0.75rem;
  color: var(--accent);
  font-weight: 600;
}

.navbar-name {
  font-size: 0.9rem;
  font-weight: 700;
}

.navbar-links {
  display: flex;
  gap: 0.4rem;
  flex-wrap: wrap;
}

.week-link {
  background: transparent;
  border: 1px solid var(--accent);
  color: var(--accent);
  padding: 0.25rem 0.6rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.85rem;
  font-family: 'Sarabun', sans-serif;
  transition: background 0.2s, color 0.2s;
}

.week-link:hover {
  background: var(--accent);
  color: var(--text);
}
</style>
