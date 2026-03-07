<script setup lang="ts">
import { ref, onMounted } from 'vue'

const sectionRef = ref<HTMLElement | null>(null)
const visible = ref(false)

function statusClass(s: string) {
  return s.toLowerCase().replace(/\s+/g, '-')
}

interface Project {
  title: string
  description: string
  tags: string[]
  status: 'Live' | 'In Progress' | 'Planned'
  href?: string
  category?: string
}

const projects: Project[] = [
  {
    title: 'huntersSHADOW',
    description:
      'A personal cybersecurity toolkit built during red team practice and ethical hacking challenges. Contains offensive security scripts, recon tools, and CTF utilities developed hands-on.',
    tags: ['Python', 'Shell', 'Red Team', 'Offensive Security'],
    status: 'Live',
    category: 'Security',
    href: 'https://github.com/HunterSreeni/huntersSHADOW',
  },
  {
    title: 'ISTQB Mock Exam Platform',
    description:
      'A fully proctored MCQ exam platform simulating the real ISTQB Foundation Level experience — timed, randomised questions, full-screen enforcement. After submission, get a detailed breakdown of every answer with explanations, score analysis, and topic-level feedback.',
    tags: ['Vue JS', 'PostgreSQL', 'Node.js', 'Exam Engine'],
    status: 'Live',
    category: 'QA',
    href: 'https://istqbmock.sreeniverse.co.in',
  },
  {
    title: 'Knowledge Orbit — Community Blog',
    description:
      'An open community blog platform where anyone can register and write posts on their favourite topic — tech, security, automation, or beyond. No gatekeeping.',
    tags: ['Vue JS', 'PostgreSQL', 'Node.js'],
    status: 'In Progress',
    category: 'Web',
  },
]

function openHref(href?: string) {
  if (href) window.open(href, '_blank', 'noopener,noreferrer')
}

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.12 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})
</script>

<template>
  <section id="projects" class="s-section" ref="sectionRef">
    <div :class="['s-fade', { visible }]">
      <p class="s-label">03 · Projects</p>
      <h2 class="s-title">Things I've Built</h2>

      <div class="grid">
        <div
          v-for="(project, i) in projects"
          :key="project.title"
          :class="['card', { clickable: !!project.href }]"
          :style="{ transitionDelay: `${i * 0.1}s` }"
          @click="openHref(project.href)"
        >
          <div class="card-top">
            <div class="card-meta">
              <span :class="['status', statusClass(project.status)]">
                {{ project.status }}
              </span>
              <span v-if="project.category" class="category">{{ project.category }}</span>
            </div>
            <span v-if="project.href" class="arrow">↗</span>
          </div>

          <h3 class="card-title">{{ project.title }}</h3>
          <p class="card-desc">{{ project.description }}</p>

          <div class="tags">
            <span v-for="tag in project.tags" :key="tag" class="tag">#{{ tag }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}

.card {
  padding: 2rem;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.018);
  transition: border-color 0.3s, background 0.3s, transform 0.3s;
}

.card.clickable {
  cursor: none;
}

.card:hover {
  border-color: rgba(255, 255, 255, 0.18);
  background: rgba(255, 255, 255, 0.045);
  transform: translateY(-6px);
}

.card-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.2rem;
}

.card-meta {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.status {
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  padding: 0.22rem 0.6rem;
  border-radius: 100px;
}

.status.live {
  background: rgba(100, 255, 150, 0.08);
  border: 1px solid rgba(100, 255, 150, 0.25);
  color: rgba(150, 255, 180, 0.85);
}

.status.in-progress {
  background: rgba(255, 210, 100, 0.08);
  border: 1px solid rgba(255, 210, 100, 0.25);
  color: rgba(255, 225, 140, 0.85);
}

.status.planned {
  background: rgba(150, 150, 255, 0.08);
  border: 1px solid rgba(150, 150, 255, 0.25);
  color: rgba(180, 180, 255, 0.85);
}

.category {
  font-family: var(--font-mono);
  font-size: 0.6rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.22);
}

.arrow {
  color: rgba(255, 255, 255, 0.2);
  font-size: 1.1rem;
  transition: color 0.2s, transform 0.2s;
}

.card:hover .arrow {
  color: white;
  transform: translate(3px, -3px);
}

.card-title {
  font-size: 1.05rem;
  font-weight: 600;
  margin-bottom: 0.75rem;
  line-height: 1.3;
}

.card-desc {
  font-size: 0.84rem;
  color: rgba(255, 255, 255, 0.47);
  line-height: 1.78;
  margin-bottom: 1.4rem;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.tag {
  font-family: var(--font-mono);
  font-size: 0.67rem;
  color: rgba(255, 255, 255, 0.28);
}
</style>
