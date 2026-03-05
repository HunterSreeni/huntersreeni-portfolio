<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const taglines = [
  'QA Engineer & SecOps Enthusiast',
  'Automation Architect',
  'Ethical Hacker in Training',
  'Bug Hunter by Day · Threat Hunter by Night',
  'Making Software Fail — So Users Don\'t Have To',
]

const displayed = ref('')
const taglineIndex = ref(0)
let timeout: ReturnType<typeof setTimeout>

function type(text: string, i = 0) {
  if (i <= text.length) {
    displayed.value = text.slice(0, i)
    timeout = setTimeout(() => type(text, i + 1), 55)
  } else {
    timeout = setTimeout(() => erase(text), 2800)
  }
}

function erase(text: string, i = text.length) {
  if (i >= 0) {
    displayed.value = text.slice(0, i)
    timeout = setTimeout(() => erase(text, i - 1), 28)
  } else {
    taglineIndex.value = (taglineIndex.value + 1) % taglines.length
    timeout = setTimeout(() => type(taglines[taglineIndex.value]), 450)
  }
}

function scrollDown() {
  document.getElementById('about')?.scrollIntoView({ behavior: 'smooth' })
}

onMounted(() => type(taglines[0]))
onUnmounted(() => clearTimeout(timeout))
</script>

<template>
  <section id="hero" class="hero">
    <div class="content">
      <p class="greeting">Hello, Universe. I'm</p>
      <h1 class="name">Sreenivasan</h1>
      <p class="tagline">
        <span>{{ displayed }}</span>
        <span class="caret">|</span>
      </p>
      <button class="cta" @click="scrollDown">
        Explore &nbsp;↓
      </button>
    </div>
    <div class="glow" />
  </section>
</template>

<style scoped>
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  position: relative;
  padding: 2rem;
}

.content {
  position: relative;
  z-index: 1;
}

.greeting {
  font-family: var(--font-mono);
  font-size: 0.82rem;
  color: rgba(255, 255, 255, 0.35);
  letter-spacing: 0.2em;
  text-transform: uppercase;
  margin-bottom: 1rem;
}

.name {
  font-size: clamp(3.2rem, 10vw, 7.5rem);
  font-weight: 700;
  letter-spacing: -0.03em;
  line-height: 1;
  margin-bottom: 1.5rem;
  background: linear-gradient(160deg, #ffffff 0%, rgba(255, 255, 255, 0.65) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.tagline {
  font-family: var(--font-mono);
  font-size: clamp(0.85rem, 2.2vw, 1.1rem);
  color: rgba(255, 255, 255, 0.5);
  min-height: 2em;
  margin-bottom: 3.5rem;
}

.caret {
  animation: blink 1s step-end infinite;
  color: rgba(255, 255, 255, 0.7);
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}

.cta {
  display: inline-flex;
  align-items: center;
  background: none;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: rgba(255, 255, 255, 0.8);
  font-family: var(--font-mono);
  font-size: 0.8rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  padding: 0.75rem 2.25rem;
  border-radius: 2px;
  cursor: pointer;
  transition: background 0.25s, border-color 0.25s, color 0.25s;
  animation: float 3s ease-in-out infinite;
}

.cta:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.4);
  color: white;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(5px); }
}

.glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 700px;
  height: 700px;
  background: radial-gradient(ellipse, rgba(180, 180, 255, 0.025) 0%, transparent 65%);
  pointer-events: none;
}
</style>
