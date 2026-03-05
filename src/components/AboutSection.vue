<script setup lang="ts">
import { ref, onMounted } from 'vue'

const sectionRef = ref<HTMLElement | null>(null)
const visible = ref(false)

interface Certificate {
  id: string
  title: string
  issuer: string
  date: string
  detail: string
  file: string
}

const certificates: Certificate[] = [
  {
    id: 'THM-D3BBGNTCPW',
    title: 'Jr Penetration Tester',
    issuer: 'TryHackMe',
    date: '4th September 2025',
    detail: '30 hours 40 minutes · Learning Path',
    file: '/certs/THM-D3BBGNTCPW.pdf',
  },
  {
    id: 'THM-P8H1M8EGOO',
    title: 'Advent of Cyber 2025',
    issuer: 'TryHackMe',
    date: '30th December 2025',
    detail: '24 cyber security challenges completed',
    file: '/certs/THM-P8H1M8EGOO.pdf',
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
  <section id="about" class="s-section" ref="sectionRef">
    <div :class="['s-fade', { visible }]">
      <p class="s-label">01 · About</p>
      <h2 class="s-title">Who Am I?</h2>

      <div class="grid">
        <!-- Bio -->
        <div class="bio">
          <p>
            Hey, I'm <strong>Sreenivasan</strong> — a QA Engineer and SecOps enthusiast
            living at the intersection of quality and security. I specialise in both manual
            and automated software testing, helping teams ship reliable, well-tested products
            that hold up under real-world pressure.
          </p>
          <p>
            Beyond testing, I have a genuine passion for cybersecurity, ethical hacking, and
            red teaming. Certified as a <strong>Jr. Penetration Tester</strong> on TryHackMe
            and constantly pushing deeper into the security space — because the best way to
            defend a system is to understand how to break it.
          </p>
          <p>
            I'm also drawn to design and development, and I care about how things look and
            feel, not just how they perform. Whether it's crafting a test strategy, automating
            a pipeline, or cracking a CTF challenge, I bring curiosity, craft, and a hacker's
            mindset to everything I do.
          </p>
        </div>

        <!-- Certs -->
        <div class="certs-col">
          <p class="certs-heading">Certifications</p>
          <div class="certs-list">
            <a
              v-for="cert in certificates"
              :key="cert.id"
              :href="cert.file"
              target="_blank"
              rel="noopener noreferrer"
              class="cert-card"
            >
              <div class="cert-badge">THM</div>
              <div class="cert-body">
                <p class="cert-title">{{ cert.title }}</p>
                <p class="cert-sub">{{ cert.issuer }} · {{ cert.date }}</p>
                <p class="cert-detail">{{ cert.detail }}</p>
              </div>
              <span class="cert-link-icon">↗</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: start;
}

.bio p {
  color: rgba(255, 255, 255, 0.6);
  line-height: 1.85;
  font-size: 0.95rem;
  margin-bottom: 1.2rem;
}

.bio p:last-child {
  margin-bottom: 0;
}

.bio strong {
  color: white;
  font-weight: 600;
}

.certs-heading {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.3);
  margin-bottom: 1rem;
}

.certs-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.cert-card {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem 1.1rem;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.02);
  text-decoration: none;
  color: inherit;
  cursor: pointer;
  transition: border-color 0.25s, background 0.25s;
}

.cert-card:hover {
  border-color: rgba(255, 255, 255, 0.18);
  background: rgba(255, 255, 255, 0.045);
}

.cert-badge {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  padding: 0.35rem 0.5rem;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.05);
  flex-shrink: 0;
  color: rgba(255, 255, 255, 0.7);
}

.cert-body {
  flex: 1;
  min-width: 0;
}

.cert-title {
  font-size: 0.88rem;
  font-weight: 600;
  margin-bottom: 0.2rem;
}

.cert-sub {
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.38);
  margin-bottom: 0.15rem;
}

.cert-detail {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.28);
  font-family: var(--font-mono);
}

.cert-link-icon {
  color: rgba(255, 255, 255, 0.25);
  font-size: 1rem;
  transition: color 0.2s, transform 0.2s;
  flex-shrink: 0;
}

.cert-card:hover .cert-link-icon {
  color: white;
  transform: translate(2px, -2px);
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: 1fr;
    gap: 2.5rem;
  }
}
</style>
