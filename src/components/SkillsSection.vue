<script setup lang="ts">
import { ref, onMounted } from 'vue'

const sectionRef = ref<HTMLElement | null>(null)
const visible = ref(false)

const skillGroups = [
  {
    label: 'Languages',
    icon: '{ }',
    note: 'Code I write',
    skills: [
      'JavaScript',
      'SQL / PostgreSQL',
      'Vue JS',
      'Python',
      'Shell · Linux',
      'PowerShell · Windows',
    ],
  },
  {
    label: 'Tools',
    icon: '⚙',
    note: 'Tools I use',
    skills: [
      'Playwright',
      'Cypress',
      'JMeter',
      'Red Team / Pen Testing',
      'n8n',
      'ngrok',
    ],
  },
  {
    label: 'AI & Automation',
    icon: '◈',
    note: 'AI I work with',
    skills: [
      'Claude · Anthropic',
      'Gemini · Google',
      'Cline (OpenRouter)',
      'Ollama · Local Models',
    ],
  },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.12 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})
</script>

<template>
  <section id="skills" class="s-section" ref="sectionRef">
    <div :class="['s-fade', { visible }]">
      <p class="s-label">02 · Skills</p>
      <h2 class="s-title">What I Work With</h2>

      <div class="grid">
        <div
          v-for="(group, i) in skillGroups"
          :key="group.label"
          class="group"
          :style="{ transitionDelay: `${i * 0.12}s` }"
        >
          <div class="group-header">
            <span class="group-icon">{{ group.icon }}</span>
            <div>
              <p class="group-label">{{ group.label }}</p>
              <p class="group-note">{{ group.note }}</p>
            </div>
          </div>

          <div class="tags">
            <span v-for="skill in group.skills" :key="skill" class="tag">
              {{ skill }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}

.group {
  padding: 1.75rem;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.018);
  transition: border-color 0.3s, background 0.3s, transform 0.3s;
}

.group:hover {
  border-color: rgba(255, 255, 255, 0.14);
  background: rgba(255, 255, 255, 0.04);
  transform: translateY(-5px);
}

.group-header {
  display: flex;
  align-items: center;
  gap: 0.9rem;
  margin-bottom: 1.4rem;
}

.group-icon {
  font-family: var(--font-mono);
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.35);
  width: 32px;
  text-align: center;
  flex-shrink: 0;
}

.group-label {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  letter-spacing: 0.13em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.5);
  margin-bottom: 0.1rem;
}

.group-note {
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.22);
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.45rem;
}

.tag {
  display: inline-block;
  font-size: 0.78rem;
  padding: 0.32rem 0.72rem;
  border: 1px solid rgba(255, 255, 255, 0.09);
  border-radius: 100px;
  color: rgba(255, 255, 255, 0.65);
  background: rgba(255, 255, 255, 0.03);
  transition: border-color 0.2s, color 0.2s, background 0.2s;
  cursor: default;
}

.tag:hover {
  border-color: rgba(255, 255, 255, 0.25);
  color: white;
  background: rgba(255, 255, 255, 0.06);
}

@media (max-width: 900px) {
  .grid {
    grid-template-columns: 1fr;
  }
}
</style>
