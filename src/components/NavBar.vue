<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const scrolled = ref(false)
const activeSection = ref('hero')
const menuOpen = ref(false)

const navLinks = [
  { id: 'about', label: 'About' },
  { id: 'skills', label: 'Skills' },
  { id: 'projects', label: 'Projects' },
  { id: 'blog', label: 'Blog' },
  { id: 'contact', label: 'Contact' },
]

function scrollTo(id: string) {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
  menuOpen.value = false
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

    <!-- Desktop links -->
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

    <!-- Mobile hamburger button -->
    <button
      class="hamburger"
      :class="{ open: menuOpen }"
      @click="menuOpen = !menuOpen"
      aria-label="Toggle menu"
    >
      <span class="bar" />
      <span class="bar" />
      <span class="bar" />
    </button>
  </nav>

  <!-- Mobile menu overlay -->
  <div :class="['mobile-menu', { open: menuOpen }]" @click="menuOpen = false">
    <div class="mobile-menu-inner" @click.stop>
      <ul class="mobile-links">
        <li v-for="link in navLinks" :key="link.id">
          <button
            :class="['mobile-link', { active: activeSection === link.id }]"
            @click="scrollTo(link.id)"
          >
            {{ link.label }}
          </button>
        </li>
      </ul>
    </div>
  </div>
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
  cursor: pointer;
  letter-spacing: 0.04em;
  font-weight: 700;
  z-index: 101;
}

@media (hover: none) and (pointer: coarse) {
  .logo { cursor: pointer; }
}

.bracket {
  color: rgba(255, 255, 255, 0.35);
}

/* Desktop links */
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
  cursor: pointer;
  padding: 0.4rem 0.8rem;
  border-radius: 4px;
  transition: color 0.2s, background 0.2s;
}

.link:hover,
.link.active {
  color: rgba(255, 255, 255, 0.9);
  background: rgba(255, 255, 255, 0.06);
}

/* Hamburger button — mobile only */
.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  z-index: 101;
}

.bar {
  display: block;
  width: 22px;
  height: 1.5px;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 2px;
  transition: transform 0.3s, opacity 0.3s;
}

.hamburger.open .bar:nth-child(1) {
  transform: translateY(6.5px) rotate(45deg);
}
.hamburger.open .bar:nth-child(2) {
  opacity: 0;
}
.hamburger.open .bar:nth-child(3) {
  transform: translateY(-6.5px) rotate(-45deg);
}

/* Mobile full-screen menu */
.mobile-menu {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 99;
  background: rgba(8, 8, 15, 0.97);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s;
}

.mobile-menu.open {
  opacity: 1;
  pointer-events: all;
}

.mobile-menu-inner {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.mobile-links {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.mobile-link {
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.5);
  font-family: var(--font-sans);
  font-size: 1.5rem;
  font-weight: 500;
  letter-spacing: 0.04em;
  cursor: pointer;
  padding: 0.6rem 1.5rem;
  border-radius: 6px;
  transition: color 0.2s, background 0.2s;
  width: 100%;
  text-align: center;
}

.mobile-link:hover,
.mobile-link.active {
  color: white;
  background: rgba(255, 255, 255, 0.06);
}

@media (max-width: 640px) {
  .navbar {
    padding: 1rem 1.25rem;
  }
  .navbar.scrolled {
    padding: 0.75rem 1.25rem;
  }
  .links {
    display: none;
  }
  .hamburger {
    display: flex;
  }
  .mobile-menu {
    display: block;
  }
}
</style>
