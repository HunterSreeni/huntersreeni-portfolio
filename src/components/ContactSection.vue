<script setup lang="ts">
import { ref, onMounted } from 'vue'

const sectionRef = ref<HTMLElement | null>(null)
const visible = ref(false)

const socials = [
  {
    icon: '✉',
    label: 'Email',
    sub: 'huntersreenihs@gmail.com',
    href: 'mailto:huntersreenihs@gmail.com',
  },
  {
    icon: '◎',
    label: 'GitHub',
    sub: 'huntersreeni',
    href: 'https://github.com/huntersreeni',
  },
  {
    icon: '◈',
    label: 'LinkedIn',
    sub: 'sreenivasan-s',
    href: 'https://www.linkedin.com/in/sreenivasan-s/',
  },
  {
    icon: '◉',
    label: 'TryHackMe',
    sub: 'huntersreenihs',
    href: 'https://tryhackme.com/p/huntersreenihs',
  },
  {
    icon: '⬡',
    label: 'Bugcrowd',
    sub: 'HunterSreeni',
    href: 'https://bugcrowd.com/h/HunterSreeni',
  },
  {
    icon: '◆',
    label: 'HackerOne',
    sub: 'huntersreeni',
    href: 'https://hackerone.com/huntersreeni',
  },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.15 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})
</script>

<template>
  <section id="contact" class="s-section contact-section" ref="sectionRef">
    <div :class="['s-fade', { visible }]">
      <p class="s-label">05 · Contact</p>
      <h2 class="s-title">Let's Connect</h2>

      <p class="tagline">
        Whether it's testing, security, a collaboration, or just saying hi
        from across the cosmos — find me at any of these coordinates.
      </p>

      <div class="socials-grid">
        <a
          v-for="s in socials"
          :key="s.label"
          :href="s.href"
          target="_blank"
          rel="noopener noreferrer"
          class="social-card"
        >
          <span class="s-icon">{{ s.icon }}</span>
          <div class="s-body">
            <p class="s-label-text">{{ s.label }}</p>
            <p class="s-sub">{{ s.sub }}</p>
          </div>
          <span class="s-arrow">↗</span>
        </a>
      </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
      <p>
        <span class="footer-star">✦</span>
        Built with curiosity by Sreenivasan · {{ new Date().getFullYear() }}
        <span class="footer-star">✦</span>
      </p>
    </footer>
  </section>
</template>

<style scoped>
.contact-section {
  padding-bottom: 4rem;
}

.tagline {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.48);
  line-height: 1.85;
  margin-bottom: 3rem;
  max-width: 560px;
}

.socials-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

.social-card {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.1rem 1.25rem;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.018);
  text-decoration: none;
  color: inherit;
  cursor: none;
  transition: border-color 0.25s, background 0.25s, transform 0.25s;
}

.social-card:hover {
  border-color: rgba(255, 255, 255, 0.18);
  background: rgba(255, 255, 255, 0.045);
  transform: translateY(-3px);
}

.s-icon {
  font-size: 1rem;
  width: 26px;
  text-align: center;
  color: rgba(255, 255, 255, 0.4);
  flex-shrink: 0;
}

.s-body {
  flex: 1;
  min-width: 0;
}

.s-label-text {
  font-size: 0.85rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.8);
  margin-bottom: 0.15rem;
}

.s-sub {
  font-family: var(--font-mono);
  font-size: 0.68rem;
  color: rgba(255, 255, 255, 0.3);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.s-arrow {
  color: rgba(255, 255, 255, 0.2);
  font-size: 0.9rem;
  flex-shrink: 0;
  transition: color 0.2s, transform 0.2s;
}

.social-card:hover .s-arrow {
  color: white;
  transform: translate(2px, -2px);
}

/* Footer */
.footer {
  margin-top: 7rem;
  padding-top: 2rem;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  text-align: center;
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.18);
}

.footer-star {
  color: rgba(255, 255, 255, 0.1);
  margin: 0 0.5rem;
}

@media (max-width: 900px) {
  .socials-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 560px) {
  .socials-grid {
    grid-template-columns: 1fr;
  }
}
</style>
