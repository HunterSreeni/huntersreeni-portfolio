<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref<HTMLCanvasElement | null>(null)

interface Star {
  x: number
  y: number
  z: number         // depth: 0 = far/small, 1 = near/large
  size: number
  baseOpacity: number
  twinklePhase: number
  twinkleSpeed: number
}

interface ShootingStar {
  x: number
  y: number
  vx: number
  vy: number
  trailLen: number
  life: number
  maxLife: number
}

let ctx: CanvasRenderingContext2D
let stars: Star[] = []
let shootingStars: ShootingStar[] = []
let mouseX = 0
let mouseY = 0
let animId: number
let shootTimer = 0

const STAR_COUNT = 280

function initStars(w: number, h: number) {
  stars = Array.from({ length: STAR_COUNT }, () => {
    const z = Math.random()
    return {
      x: Math.random() * w,
      y: Math.random() * h,
      z,
      size: z * 1.8 + 0.2,
      baseOpacity: Math.random() * 0.5 + 0.2,
      twinklePhase: Math.random() * Math.PI * 2,
      twinkleSpeed: Math.random() * 0.025 + 0.005,
    }
  })
}

function spawnShootingStar(w: number, h: number) {
  const angle = (Math.random() * 25 + 15) * (Math.PI / 180)
  const speed = Math.random() * 9 + 7
  shootingStars.push({
    x: Math.random() * w * 0.75,
    y: Math.random() * h * 0.35,
    vx: Math.cos(angle) * speed,
    vy: Math.sin(angle) * speed,
    trailLen: Math.random() * 90 + 70,
    life: 0,
    maxLife: Math.random() * 50 + 45,
  })
}

function draw() {
  if (!ctx || !canvas.value) return

  const w = canvas.value.width
  const h = canvas.value.height

  ctx.clearRect(0, 0, w, h)

  const mx = mouseX / w - 0.5
  const my = mouseY / h - 0.5

  // --- Stars ---
  const constellationCandidates: { px: number; py: number }[] = []

  for (const star of stars) {
    star.twinklePhase += star.twinkleSpeed
    const twinkle = Math.sin(star.twinklePhase) * 0.3 + 0.7

    // Parallax: far stars move less
    const px = star.x + mx * star.z * 35
    const py = star.y + my * star.z * 35

    const alpha = star.baseOpacity * twinkle

    // Far stars slightly cooler (blue-ish white), near stars warm white
    const r = Math.floor(200 + star.z * 55)
    const g = Math.floor(210 + star.z * 45)

    ctx.beginPath()
    ctx.arc(px, py, star.size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(${r}, ${g}, 255, ${alpha})`
    ctx.fill()

    // Glow on bigger stars
    if (star.size > 1.2) {
      ctx.beginPath()
      ctx.arc(px, py, star.size * 2.5, 0, Math.PI * 2)
      ctx.fillStyle = `rgba(200, 220, 255, ${alpha * 0.08})`
      ctx.fill()
    }

    // Collect nearby stars for constellation effect
    const dx = px - mouseX
    const dy = py - mouseY
    if (Math.sqrt(dx * dx + dy * dy) < 130) {
      constellationCandidates.push({ px, py })
    }
  }

  // --- Constellation lines near cursor ---
  for (let i = 0; i < constellationCandidates.length; i++) {
    for (let j = i + 1; j < constellationCandidates.length; j++) {
      const a = constellationCandidates[i]
      const b = constellationCandidates[j]
      const dx = a.px - b.px
      const dy = a.py - b.py
      const dist = Math.sqrt(dx * dx + dy * dy)
      if (dist < 90) {
        const alpha = (1 - dist / 90) * 0.25
        ctx.beginPath()
        ctx.moveTo(a.px, a.py)
        ctx.lineTo(b.px, b.py)
        ctx.strokeStyle = `rgba(255, 255, 255, ${alpha})`
        ctx.lineWidth = 0.5
        ctx.stroke()
      }
    }
  }

  // --- Shooting stars ---
  shootTimer++
  if (shootTimer > 140 + Math.random() * 120) {
    spawnShootingStar(w, h)
    shootTimer = 0
  }

  shootingStars = shootingStars.filter((s) => s.life < s.maxLife)

  for (const s of shootingStars) {
    s.life++
    s.x += s.vx
    s.y += s.vy

    const progress = s.life / s.maxLife
    const opacity = progress < 0.3
      ? progress / 0.3
      : 1 - (progress - 0.3) / 0.7

    const tailX = s.x - s.vx * (s.trailLen / s.vx)
    const tailY = s.y - s.vy * (s.trailLen / s.vy)

    const grad = ctx.createLinearGradient(tailX, tailY, s.x, s.y)
    grad.addColorStop(0, `rgba(255, 255, 255, 0)`)
    grad.addColorStop(1, `rgba(255, 255, 255, ${opacity * 0.85})`)

    ctx.beginPath()
    ctx.moveTo(tailX, tailY)
    ctx.lineTo(s.x, s.y)
    ctx.strokeStyle = grad
    ctx.lineWidth = 1.5
    ctx.stroke()

    // Tip sparkle
    ctx.beginPath()
    ctx.arc(s.x, s.y, 1.5, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(255, 255, 255, ${opacity})`
    ctx.fill()
  }

  animId = requestAnimationFrame(draw)
}

function resize() {
  if (!canvas.value) return
  canvas.value.width = window.innerWidth
  canvas.value.height = window.innerHeight
  initStars(canvas.value.width, canvas.value.height)
}

function onMouseMove(e: MouseEvent) {
  mouseX = e.clientX
  mouseY = e.clientY
}

onMounted(() => {
  if (!canvas.value) return
  ctx = canvas.value.getContext('2d')!
  resize()
  window.addEventListener('resize', resize)
  window.addEventListener('mousemove', onMouseMove)
  animId = requestAnimationFrame(draw)
})

onUnmounted(() => {
  cancelAnimationFrame(animId)
  window.removeEventListener('resize', resize)
  window.removeEventListener('mousemove', onMouseMove)
})
</script>

<template>
  <canvas ref="canvas" class="star-canvas" />
</template>

<style scoped>
.star-canvas {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  background: radial-gradient(ellipse at 50% 40%, #0c0c1e 0%, #08080f 70%);
}
</style>
