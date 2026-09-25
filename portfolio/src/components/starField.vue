<!-- SpaceBackground.vue -->
<template>
  <canvas ref="canvasEl" class="space-canvas"></canvas>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'



const canvasEl = ref(null)
let ctx, animationId
let width, height
let time = 0

let stars = []
let nebulae = []
let shootingStar = null
let nextShootingStarTime = 0

let mouseX = 0, mouseY = 0
let targetParallaxX = 0, targetParallaxY = 0
let parallaxX = 0, parallaxY = 0

// ---------- STARS ----------
function initStars() {
  stars = []
  const count = Math.floor((width * height) / 3200)
  for (let i = 0; i < count; i++) {
    const layer = Math.random()
    let depth, sizeRange, opacityRange
    if (layer < 0.6) { depth = 0.15; sizeRange = [0.3, 0.8]; opacityRange = [0.15, 0.35] }      // far
    else if (layer < 0.9) { depth = 0.4; sizeRange = [0.6, 1.3]; opacityRange = [0.3, 0.55] }   // mid
    else { depth = 0.8; sizeRange = [1.2, 2.2]; opacityRange = [0.5, 0.9] }                      // near

    stars.push({
      x: Math.random() * width,
      y: Math.random() * height,
      size: Math.random() * (sizeRange[1] - sizeRange[0]) + sizeRange[0],
      baseOpacity: Math.random() * (opacityRange[1] - opacityRange[0]) + opacityRange[0],
      twinkleSpeed: Math.random() * 0.015 + 0.004,
      twinklePhase: Math.random() * Math.PI * 2,
      depth,
      blink: 0
    })
  }
}

function maybeTriggerBlink() {
  if (Math.random() < 0.03) {
    const star = stars[Math.floor(Math.random() * stars.length)]
    if (star.blink === 0) star.blink = 1
  }
}

// ---------- NEBULAE (slow drifting color clouds — pure space vibe) ----------
function initNebulae() {
  const colors = [
    { r: 99, g: 102, b: 241 },   // indigo
    { r: 56, g: 189, b: 248 },   // cyan
    { r: 168, g: 85, b: 247 },   // violet
    { r: 244, g: 114, b: 182 },  // pink
    { r: 52, g: 211, b: 153 },   // teal/green
    { r: 251, g: 191, b: 36 }    // amber
  ]
  nebulae = colors.map((c) => ({
    x: Math.random() * width,
    y: Math.random() * height,
    baseRadius: Math.random() * 280 + 320,
    rgb: c,
    pulsePhase: Math.random() * Math.PI * 2,
    pulseSpeed: 0.0015 + Math.random() * 0.001,
    driftAngle: Math.random() * Math.PI * 2,
    driftRadius: Math.random() * 25 + 15,
    driftSpeed: 0.0002 + Math.random() * 0.0002
  }))
}

function drawNebulae() {
  nebulae.forEach((n) => {
    n.pulsePhase += n.pulseSpeed
    n.driftAngle += n.driftSpeed

    const pulse = Math.sin(n.pulsePhase) * 0.5 + 0.5
    const radius = n.baseRadius * (0.9 + pulse * 0.15)
    const alpha = 0.05 + pulse * 0.04

    const nx = n.x + Math.cos(n.driftAngle) * n.driftRadius
    const ny = n.y + Math.sin(n.driftAngle) * n.driftRadius

    const grad = ctx.createRadialGradient(nx, ny, 0, nx, ny, radius)
    grad.addColorStop(0, `rgba(${n.rgb.r}, ${n.rgb.g}, ${n.rgb.b}, ${alpha})`)
    grad.addColorStop(1, 'rgba(0,0,0,0)')
    ctx.fillStyle = grad
    ctx.beginPath()
    ctx.arc(nx, ny, radius, 0, Math.PI * 2)
    ctx.fill()
  })
}

// ---------- SHOOTING STAR (fires every 5-10s) ----------
function scheduleNextShootingStar() {
  const delaySeconds = Math.random() * 5 + 5 // 5s to 10s
  nextShootingStarTime = performance.now() + delaySeconds * 1000
}

function maybeSpawnShootingStar() {
  if (!shootingStar && performance.now() >= nextShootingStarTime) {
    shootingStar = {
      x: Math.random() * width * 0.6,
      y: Math.random() * height * 0.3,
      len: 90,
      speed: 9,
      angle: Math.PI / 5,
      life: 1
    }
    scheduleNextShootingStar()
  }
}

function drawShootingStar() {
  if (!shootingStar) return
  const s = shootingStar
  const dx = Math.cos(s.angle) * s.len
  const dy = Math.sin(s.angle) * s.len
  const grad = ctx.createLinearGradient(s.x, s.y, s.x - dx, s.y - dy)
  grad.addColorStop(0, `rgba(255,255,255,${s.life})`)
  grad.addColorStop(1, 'rgba(255,255,255,0)')
  ctx.strokeStyle = grad
  ctx.lineWidth = 1.5
  ctx.beginPath()
  ctx.moveTo(s.x, s.y)
  ctx.lineTo(s.x - dx, s.y - dy)
  ctx.stroke()

  s.x += Math.cos(s.angle) * s.speed
  s.y += Math.sin(s.angle) * s.speed
  s.life -= 0.015
  if (s.life <= 0) shootingStar = null
}

// ---------- SETUP / RESIZE / MOUSE ----------
function resize() {
  width = canvasEl.value.width = window.innerWidth
  height = canvasEl.value.height = window.innerHeight
  initStars()
  initNebulae()
}

function handleMouseMove(e) {
  mouseX = e.clientX
  mouseY = e.clientY
  targetParallaxX = (e.clientX - width / 2) * 0.05
  targetParallaxY = (e.clientY - height / 2) * 0.05
}

// ---------- MAIN DRAW LOOP ----------
function draw() {
  time += 1
  ctx.clearRect(0, 0, width, height)

  parallaxX += (targetParallaxX - parallaxX) * 0.06
  parallaxY += (targetParallaxY - parallaxY) * 0.06

  drawNebulae()
  maybeTriggerBlink()

  for (const star of stars) {
    const twinkle = Math.sin(time * star.twinkleSpeed + star.twinklePhase) * 0.5 + 0.5
    let opacity = star.baseOpacity * (0.5 + twinkle * 0.5)
    let size = star.size

    if (star.blink > 0) {
      const flashCurve = Math.sin(star.blink * Math.PI)
      opacity = Math.min(1, opacity + flashCurve * 0.9)
      size = star.size + flashCurve * 2.2
      star.blink += 0.04
      if (star.blink >= 1) star.blink = 0
    }

    const sx = star.x + parallaxX * star.depth
    const sy = star.y + parallaxY * star.depth

    const dist = Math.hypot(mouseX - sx, mouseY - sy)
    const proximity = Math.max(0, 1 - dist / 130)
    size += proximity * 1.8
    opacity = Math.min(1, opacity + proximity * 0.6)

    ctx.beginPath()
    ctx.arc(sx, sy, size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(255, 255, 255, ${opacity})`
    ctx.fill()

    if (star.depth > 0.6 || star.blink > 0) {
      ctx.shadowBlur = star.blink > 0 ? 10 : 6
      ctx.shadowColor = `rgba(255,255,255,${opacity * 0.6})`
    } else {
      ctx.shadowBlur = 0
    }
  }

  maybeSpawnShootingStar()
  drawShootingStar()

  animationId = requestAnimationFrame(draw)
}

onMounted(() => {
  ctx = canvasEl.value.getContext('2d')
  resize()
  scheduleNextShootingStar()
  draw()
  window.addEventListener('resize', resize)
  window.addEventListener('mousemove', handleMouseMove)
})

onUnmounted(() => {
  cancelAnimationFrame(animationId)
  window.removeEventListener('resize', resize)
  window.removeEventListener('mousemove', handleMouseMove)
})
</script>

<style scoped>
.space-canvas {
  position: fixed;
  inset: 0;
  z-index: -1;
  background: radial-gradient(ellipse at center, #141425 0%, #050508 75%);
}

</style>
