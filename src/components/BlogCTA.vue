<script setup lang="ts">
import { ref, onMounted } from 'vue'

const sectionRef = ref<HTMLElement | null>(null)
const visible = ref(false)

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) visible.value = true },
    { threshold: 0.2 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})
</script>

<template>
  <section id="blog" class="s-section" ref="sectionRef">
    <div :class="['s-fade', { visible }]">
      <p class="s-label">04 · Blog</p>

      <div class="card">
        <!-- Left content -->
        <div class="left">
          <span class="badge">Community Platform</span>
          <h2 class="title">Knowledge Orbit</h2>
          <p class="desc">
            A community-driven blog where curious minds share what they know.
            Register, pick a topic — tech, cybersecurity, automation, design,
            anything — and write. Open to everyone. No gatekeeping.
          </p>

          <ul class="features">
            <li>✦ &nbsp;Open registration — anyone can join</li>
            <li>✦ &nbsp;Write about any topic you're passionate about</li>
            <li>✦ &nbsp;Community moderated, community driven</li>
          </ul>

          <div class="actions">
            <button class="btn-primary" disabled>
              Visit Blog
            </button>
            <span class="wip-tag">In development</span>
          </div>
        </div>

        <!-- Decorative orbit animation -->
        <div class="orbit-wrap" aria-hidden="true">
          <div class="ring r1" />
          <div class="ring r2" />
          <div class="ring r3" />
          <div class="dot d1" />
          <div class="dot d2" />
          <div class="dot d3" />
          <div class="center-star">✦</div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.card {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 3rem;
  align-items: center;
  padding: 3rem;
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 18px;
  background: rgba(255, 255, 255, 0.018);
}

.badge {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: 0.62rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 0.28rem 0.7rem;
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 100px;
  color: rgba(255, 255, 255, 0.4);
  margin-bottom: 1.1rem;
}

.title {
  font-size: clamp(1.8rem, 3.5vw, 2.6rem);
  font-weight: 700;
  letter-spacing: -0.02em;
  margin-bottom: 1rem;
}

.desc {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.52);
  line-height: 1.8;
  margin-bottom: 1.5rem;
  max-width: 500px;
}

.features {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 2rem;
}

.features li {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.38);
}

.actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.btn-primary {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.14);
  color: rgba(255, 255, 255, 0.5);
  font-family: var(--font-mono);
  font-size: 0.78rem;
  letter-spacing: 0.08em;
  padding: 0.7rem 1.5rem;
  border-radius: 4px;
  cursor: not-allowed;
}

.wip-tag {
  font-family: var(--font-mono);
  font-size: 0.68rem;
  color: rgba(255, 210, 100, 0.55);
  border: 1px solid rgba(255, 210, 100, 0.2);
  padding: 0.2rem 0.5rem;
  border-radius: 3px;
}

/* --- Orbit visual --- */
.orbit-wrap {
  position: relative;
  width: 200px;
  height: 200px;
  flex-shrink: 0;
}

.ring {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.06);
}

.r1 { width: 56px;  height: 56px; }
.r2 { width: 110px; height: 110px; }
.r3 {
  width: 170px;
  height: 170px;
  border-style: dashed;
  border-color: rgba(255, 255, 255, 0.04);
  animation: spin 22s linear infinite;
}

@keyframes spin {
  to { transform: translate(-50%, -50%) rotate(360deg); }
}

.dot {
  position: absolute;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 50%;
  top: 50%;
  left: 50%;
}

.d1 {
  width: 7px;
  height: 7px;
  margin: -3.5px;
  animation: orbit 7s linear infinite;
  transform-origin: 0 0;
}

.d2 {
  width: 5px;
  height: 5px;
  margin: -2.5px;
  opacity: 0.5;
  animation: orbit 11s linear infinite reverse;
  transform-origin: 0 0;
}

.d3 {
  width: 4px;
  height: 4px;
  margin: -2px;
  opacity: 0.3;
  animation: orbit3 16s linear infinite;
  transform-origin: 0 0;
}

@keyframes orbit {
  from { transform: rotate(0deg) translateX(55px); }
  to   { transform: rotate(360deg) translateX(55px); }
}

@keyframes orbit3 {
  from { transform: rotate(0deg) translateX(85px); }
  to   { transform: rotate(360deg) translateX(85px); }
}

.center-star {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 1.3rem;
  color: rgba(255, 255, 255, 0.55);
  animation: pulse 3.5s ease infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 0.55; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 0.9; transform: translate(-50%, -50%) scale(1.15); }
}

@media (max-width: 768px) {
  .card {
    grid-template-columns: 1fr;
    padding: 2rem;
  }
  .orbit-wrap {
    display: none;
  }
}
</style>
