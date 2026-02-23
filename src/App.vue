<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import StarCanvas from './components/StarCanvas.vue'
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import SkillsSection from './components/SkillsSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import BlogCTA from './components/BlogCTA.vue'
import ContactSection from './components/ContactSection.vue'

const cursorX = ref(0)
const cursorY = ref(0)
const ringX = ref(0)
const ringY = ref(0)
const cursorVisible = ref(false)

let animFrame: number

const lerp = (a: number, b: number, t: number) => a + (b - a) * t

function onMouseMove(e: MouseEvent) {
  cursorX.value = e.clientX
  cursorY.value = e.clientY
  cursorVisible.value = true
}

function animateCursor() {
  ringX.value = lerp(ringX.value, cursorX.value, 0.1)
  ringY.value = lerp(ringY.value, cursorY.value, 0.1)
  animFrame = requestAnimationFrame(animateCursor)
}

onMounted(() => {
  window.addEventListener('mousemove', onMouseMove)
  animateCursor()
})

onUnmounted(() => {
  window.removeEventListener('mousemove', onMouseMove)
  cancelAnimationFrame(animFrame)
})
</script>

<template>
  <div>
    <!-- Custom cursor -->
    <div
      class="cursor-dot"
      :style="{ left: cursorX + 'px', top: cursorY + 'px', opacity: cursorVisible ? 1 : 0 }"
    />
    <div
      class="cursor-ring"
      :style="{ left: ringX + 'px', top: ringY + 'px', opacity: cursorVisible ? 1 : 0 }"
    />

    <!-- Star field background -->
    <StarCanvas />

    <!-- Navigation -->
    <NavBar />

    <!-- Content -->
    <main>
      <HeroSection />
      <AboutSection />
      <SkillsSection />
      <ProjectsSection />
      <BlogCTA />
      <ContactSection />
    </main>
  </div>
</template>
