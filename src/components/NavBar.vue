<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const scrolled = ref(false)
const activeSection = ref('hero')

const navLinks = [
  { id: 'about', label: 'About' },
  { id: 'skills', label: 'Skills' },
  { id: 'projects', label: 'Projects' },
  { id: 'blog', label: 'Blog' },
  { id: 'contact', label: 'Contact' },
]

function scrollTo(id: string) {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
}

function onScroll() {
  scrolled.value = window.scrollY > 60

  const ids = ['hero', 'about', 'skills', 'projects', 'blog', 'contact']
  for (const id of [...ids].reverse()) {
    const el = document.getElementById(id)
    if (el && window.scrollY >= el.offsetTop - 250) {
      activeSection.value = id
      break
    }
  }
}

onMounted(() => window.addEventListener('scroll', onScroll, { passive: true }))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<template>
  <nav :class="['navbar', { scrolled }]">
    <button class="logo" @click="scrollTo('hero')">
      <span class="bracket">&lt;</span>S<span class="bracket">/&gt;</span>
    </button>

    <ul class="links">
      <li v-for="link in navLinks" :key="link.id">
        <button
          :class="['link', { active: activeSection === link.id }]"
          @click="scrollTo(link.id)"
        >
          {{ link.label }}
        </button>
      </li>
    </ul>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 3rem;
  transition: background 0.4s, backdrop-filter 0.4s, padding 0.3s, border-color 0.4s;
  border-bottom: 1px solid transparent;
}

.navbar.scrolled {
  background: rgba(8, 8, 15, 0.75);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  padding: 1rem 3rem;
  border-bottom-color: rgba(255, 255, 255, 0.05);
}

.logo {
  font-family: var(--font-mono);
  font-size: 1rem;
  color: white;
  background: none;
  border: none;
  cursor: none;
  letter-spacing: 0.04em;
  font-weight: 700;
}

.bracket {
  color: rgba(255, 255, 255, 0.35);
}

.links {
  display: flex;
  gap: 0.25rem;
  list-style: none;
}

.link {
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.45);
  font-family: var(--font-sans);
  font-size: 0.82rem;
  letter-spacing: 0.04em;
  cursor: none;
  padding: 0.4rem 0.8rem;
  border-radius: 4px;
  transition: color 0.2s, background 0.2s;
}

.link:hover,
.link.active {
  color: rgba(255, 255, 255, 0.9);
  background: rgba(255, 255, 255, 0.06);
}

@media (max-width: 600px) {
  .navbar {
    padding: 1rem 1.25rem;
  }
  .navbar.scrolled {
    padding: 0.75rem 1.25rem;
  }
  .link {
    font-size: 0.75rem;
    padding: 0.35rem 0.5rem;
  }
}
</style>
