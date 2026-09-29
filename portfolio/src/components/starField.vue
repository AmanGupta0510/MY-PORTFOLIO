SpaceBackground.vue
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

<!-- <template>
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
let shootingStars = []
let particles = []           // fragments, click sparkles and cursor dust all live here
let nextShootingStarTime = 0

let mouseX = -9999, mouseY = -9999   // off-screen until the mouse actually moves
let targetParallaxX = 0, targetParallaxY = 0
let parallaxX = 0, parallaxY = 0

const MAX_SHOOTING = 3
const SHOOT_MIN = 1.2   // seconds between shooting stars
const SHOOT_MAX = 3.5

// ---------- PARTICLES (shared by fragments, sparkles, dust) ----------
function addParticle(x, y, vx, vy, size, color, decay, friction = 0.96) {
  if (particles.length > 300) return
  particles.push({ x, y, vx, vy, size, color, decay, friction, life: 1 })
}

function drawParticles() {
  for (const p of particles) {
    p.x += p.vx
    p.y += p.vy
    p.vx *= p.friction
    p.vy *= p.friction
    p.life -= p.decay

    ctx.beginPath()
    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(${p.color}, ${Math.max(0, p.life)})`
    ctx.fill()
  }
  particles = particles.filter((p) => p.life > 0)
}

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
      blink: 0,
      ox: 0,   // push-away offset from the cursor
      oy: 0
    })
  }
}

function maybeTriggerBlink() {
  if (Math.random() < 0.05) {
    const star = stars[Math.floor(Math.random() * stars.length)]
    if (star.blink === 0) star.blink = 1
  }
}

// ---------- NEBULAE (slow drifting clouds, infrared tones included) ----------
function initNebulae() {
  const colors = [
    { r: 99, g: 102, b: 241 },   // indigo
    { r: 56, g: 189, b: 248 },   // cyan
    { r: 168, g: 85, b: 247 },   // violet
    { r: 244, g: 114, b: 182 },  // pink
    { r: 239, g: 68, b: 68 },    // infrared red
    { r: 251, g: 113, b: 60 },   // ember orange
    { r: 217, g: 70, b: 239 }    // magenta
  ]
  nebulae = colors.map((c) => ({
    x: Math.random() * width,
    y: Math.random() * height,
    baseRadius: Math.random() * 280 + 220,
    rgb: c,
    depth: Math.random() * 0.3 + 0.2,
    pulsePhase: Math.random() * Math.PI * 2,
    pulseSpeed: 0.0015 + Math.random() * 0.001,
    driftAngle: Math.random() * Math.PI * 2,
    driftRadius: Math.random() * 25 + 15,
    driftSpeed: 0.0002 + Math.random() * 0.0002
  }))
}

function drawNebulae() {
  ctx.globalCompositeOperation = 'lighter'   // overlapping clouds add light, like real emission nebulae

  nebulae.forEach((n) => {
    n.pulsePhase += n.pulseSpeed
    n.driftAngle += n.driftSpeed

    const pulse = Math.sin(n.pulsePhase) * 0.5 + 0.5
    const radius = n.baseRadius * (0.9 + pulse * 0.15)

    const nx = n.x + Math.cos(n.driftAngle) * n.driftRadius + parallaxX * n.depth
    const ny = n.y + Math.sin(n.driftAngle) * n.driftRadius + parallaxY * n.depth

    // glow a little brighter when the cursor is over the cloud
    const dist = Math.hypot(mouseX - nx, mouseY - ny)
    const near = Math.max(0, 1 - dist / (radius * 0.9))
    const alpha = 0.045 + pulse * 0.035 + near * 0.05

    const grad = ctx.createRadialGradient(nx, ny, 0, nx, ny, radius)
    grad.addColorStop(0, `rgba(${n.rgb.r}, ${n.rgb.g}, ${n.rgb.b}, ${alpha})`)
    grad.addColorStop(1, 'rgba(0,0,0,0)')
    ctx.fillStyle = grad
    ctx.beginPath()
    ctx.arc(nx, ny, radius, 0, Math.PI * 2)
    ctx.fill()
  })

  ctx.globalCompositeOperation = 'source-over'
}

// ---------- SHOOTING STARS ----------
function scheduleNextShootingStar() {
  const delay = Math.random() * (SHOOT_MAX - SHOOT_MIN) + SHOOT_MIN
  nextShootingStarTime = performance.now() + delay * 1000
}

function maybeSpawnShootingStar() {
  if (performance.now() < nextShootingStarTime) return

  if (shootingStars.length < MAX_SHOOTING) {
    let angle = Math.PI / 6 + Math.random() * (Math.PI / 6)   // down-right
    if (Math.random() < 0.3) angle = Math.PI - angle            // sometimes down-left

    shootingStars.push({
      x: Math.random() * width,
      y: Math.random() * height * 0.35,
      len: Math.random() * 90 + 90,
      speed: Math.random() * 7 + 10,
      angle,
      life: 1,
      decay: 0.010 + Math.random() * 0.006,
      willBreak: Math.random() < 0.4,
      hasBroken: false
    })
  }
  scheduleNextShootingStar()
}

function breakStar(s) {
  const count = Math.floor(Math.random() * 4) + 4
  for (let i = 0; i < count; i++) {
    const a = s.angle + (Math.random() - 0.5) * 1.4
    const sp = Math.random() * 3 + 1
    addParticle(s.x, s.y, Math.cos(a) * sp, Math.sin(a) * sp, Math.random() * 1.4 + 0.5, '255, 235, 210', 0.03)
  }
}

function drawShootingStars() {
  shootingStars.forEach((s) => {
    const dx = Math.cos(s.angle) * s.len
    const dy = Math.sin(s.angle) * s.len

    const grad = ctx.createLinearGradient(s.x, s.y, s.x - dx, s.y - dy)
    grad.addColorStop(0, `rgba(255, 255, 255, ${s.life})`)
    grad.addColorStop(0.3, `rgba(180, 210, 255, ${s.life * 0.5})`)
    grad.addColorStop(1, 'rgba(180, 210, 255, 0)')
    ctx.strokeStyle = grad
    ctx.lineWidth = 1.8
    ctx.lineCap = 'round'
    ctx.beginPath()
    ctx.moveTo(s.x, s.y)
    ctx.lineTo(s.x - dx, s.y - dy)
    ctx.stroke()

    // bright head
    ctx.beginPath()
    ctx.arc(s.x, s.y, 1.8, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(255, 255, 255, ${s.life})`
    ctx.fill()

    s.x += Math.cos(s.angle) * s.speed
    s.y += Math.sin(s.angle) * s.speed
    s.life -= s.decay

    if (s.willBreak && !s.hasBroken && s.life < 0.55) {
      breakStar(s)
      s.hasBroken = true
      s.decay *= 2.5   // the main streak dies out quickly once it breaks up
    }
  })

  shootingStars = shootingStars.filter(
    (s) => s.life > 0 && s.x > -200 && s.x < width + 200 && s.y < height + 200
  )
}

// ---------- INTERACTION ----------
function burstAt(x, y) {
  const colors = ['255,255,255', '125,211,252', '196,181,253', '253,164,175']
  const count = 16
  for (let i = 0; i < count; i++) {
    const a = ((Math.PI * 2) / count) * i + Math.random() * 0.3
    const sp = Math.random() * 3 + 1.5
    addParticle(
      x, y,
      Math.cos(a) * sp, Math.sin(a) * sp,
      Math.random() * 1.6 + 0.8,
      colors[Math.floor(Math.random() * colors.length)],
      0.025,
      0.95
    )
  }
}

function handleMouseMove(e) {
  mouseX = e.clientX
  mouseY = e.clientY
  targetParallaxX = (e.clientX - width / 2) * 0.05
  targetParallaxY = (e.clientY - height / 2) * 0.05

  // cursor stardust
  if (Math.random() < 0.6) {
    addParticle(
      e.clientX, e.clientY,
      (Math.random() - 0.5) * 0.6,
      (Math.random() - 0.5) * 0.6 + 0.2,
      Math.random() * 1.4 + 0.4,
      '196, 181, 253',
      0.02,
      0.98
    )
  }
}

function handleClick(e) {
  burstAt(e.clientX, e.clientY)
}

function handleMouseOut(e) {
  if (!e.relatedTarget) {   // cursor left the window
    mouseX = -9999
    mouseY = -9999
  }
}

// ---------- SETUP ----------
function resize() {
  width = canvasEl.value.width = window.innerWidth
  height = canvasEl.value.height = window.innerHeight
  initStars()
  initNebulae()
}

// ---------- MAIN LOOP ----------
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
    let flash = 0

    if (star.blink > 0) {
      flash = Math.sin(star.blink * Math.PI)
      opacity = Math.min(1, opacity + flash * 0.9)
      size = star.size + flash * 2.2
      star.blink += 0.04
      if (star.blink >= 1) star.blink = 0
    }

    // base position with parallax
    const bx = star.x + parallaxX * star.depth
    const by = star.y + parallaxY * star.depth

    // distance to cursor
    const dx = bx - mouseX
    const dy = by - mouseY
    const dist = Math.hypot(dx, dy)
    const proximity = Math.max(0, 1 - dist / 130)

    // gentle push away from the cursor — nearer stars move more
    const push = Math.max(0, 1 - dist / 110)
    const targetOx = dist > 0 ? (dx / dist) * push * 30 * star.depth : 0
    const targetOy = dist > 0 ? (dy / dist) * push * 30 * star.depth : 0
    star.ox += (targetOx - star.ox) * 0.08
    star.oy += (targetOy - star.oy) * 0.08

    const sx = bx + star.ox
    const sy = by + star.oy

    size += proximity * 1.8
    opacity = Math.min(1, opacity + proximity * 0.6)

    // glow is set BEFORE the fill so it applies to this star
    if (star.depth > 0.6 || flash > 0 || proximity > 0.3) {
      ctx.shadowBlur = flash > 0 ? 10 : 6
      ctx.shadowColor = `rgba(255, 255, 255, ${opacity * 0.6})`
    } else {
      ctx.shadowBlur = 0
    }

    ctx.beginPath()
    ctx.arc(sx, sy, size, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(255, 255, 255, ${opacity})`
    ctx.fill()

    // cross flare at the peak of a blink
    if (flash > 0.5) {
      const l = size * 3.5 * flash
      ctx.strokeStyle = `rgba(255, 255, 255, ${opacity * 0.5})`
      ctx.lineWidth = 0.8
      ctx.beginPath()
      ctx.moveTo(sx - l, sy)
      ctx.lineTo(sx + l, sy)
      ctx.moveTo(sx, sy - l)
      ctx.lineTo(sx, sy + l)
      ctx.stroke()
    }
  }
  ctx.shadowBlur = 0   // reset so nothing else drawn this frame inherits the glow

  maybeSpawnShootingStar()
  drawShootingStars()
  drawParticles()

  animationId = requestAnimationFrame(draw)
}

onMounted(() => {
  ctx = canvasEl.value.getContext('2d')
  resize()
  scheduleNextShootingStar()
  draw()
  window.addEventListener('resize', resize)
  window.addEventListener('mousemove', handleMouseMove)
  window.addEventListener('click', handleClick)
  window.addEventListener('mouseout', handleMouseOut)
})

onUnmounted(() => {
  cancelAnimationFrame(animationId)
  window.removeEventListener('resize', resize)
  window.removeEventListener('mousemove', handleMouseMove)
  window.removeEventListener('click', handleClick)
  window.removeEventListener('mouseout', handleMouseOut)
})
</script>

<style scoped>
.space-canvas {
  position: fixed;
  inset: 0;
  z-index: 0;
  background: radial-gradient(ellipse at center, #000000 0%, #0c0c19 75%);
}
</style> -->
