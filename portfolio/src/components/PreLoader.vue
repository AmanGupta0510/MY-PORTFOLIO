<!-- Preloader.vue -->
<template>
  <div v-if="showLoader" class="preloader" ref="loaderEl">
     <p class="loading-text" ref="textEl">
      <span
        v-for="(char, i) in 'Loading...'.split('')"
        :key="i"
        class="char"
      >{{ char === ' ' ? '\u00A0' : char }}</span>
    </p>

    <svg class="rocket" ref="rocketEl" viewBox="0 0 60 120" v-show="showRocket">
      <ellipse cx="30" cy="45" rx="14" ry="35" fill="#e2e8f0" />
      <polygon points="30,0 44,45 16,45" fill="#94a3b8" />
      <circle cx="30" cy="35" r="7" fill="#7dd3fc" opacity="0.8" />
      <polygon points="16,60 4,85 16,75" fill="#c4b5fd" />
      <polygon points="44,60 56,85 44,75" fill="#c4b5fd" />
      <polygon class="flame" points="22,78 30,110 38,78" fill="#fbbf24" />
    </svg>

    <div class="exhaust-wrap" ref="exhaustWrap"></div>
  </div>


</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'

const showLoader = ref(true)
const showRocket = ref(false)
const loaderEl = ref(null)
const textEl = ref(null)
const rocketEl = ref(null)
const exhaustWrap = ref(null)




const emit = defineEmits(['done'])
let exhaustInterval = null

function spawnExhaustParticle() {
  const rocketRect = rocketEl.value.getBoundingClientRect()
  const p = document.createElement('div')
  p.className = 'exhaust-particle'
  p.style.left = rocketRect.left + rocketRect.width / 2 + (Math.random() * 20 - 10) + 'px'
  p.style.top = rocketRect.bottom - 10 + 'px'
  exhaustWrap.value.appendChild(p)

  gsap.to(p, {
    y: 40 + Math.random() * 50,
    x: (Math.random() - 0.5) * 40,
    opacity: 0,
    scale: 0.3,
    duration: 0.6,
    ease: 'power1.out',
    onComplete: () => p.remove()
  })
}

onMounted(() => {

  document.body.style.overflow = 'hidden'
  const tl = gsap.timeline({ onComplete: () => {
    document.body.style.overflow = ''
    emit('done')
  }
  })

  // Phase 1: config lines type in one by one

   tl.from('.char', {
    opacity: 0,
    y: 14,
    duration: 0.4,
    stagger: 0.04,
    ease: 'back.out(1.6)'
  })
  // hold briefly, then wave out the same way
  tl.to('.char', {
    opacity: 0,
    y: -10,
    duration: 0.3,
    stagger: 0.03,
    ease: 'power1.in'
  }, '+=0.3')

  // Phase 2: rocket launches
  .call(() => {
    showRocket.value = true
    exhaustInterval = setInterval(spawnExhaustParticle, 60)
  })
  .fromTo(rocketEl.value,
    { y: 0, opacity: 1 },
    { y: '-120vh', duration: 1.4, ease: 'power2.in', rotation: 3 }
  )
  .call(() => clearInterval(exhaustInterval))


  .to(loaderEl.value, { opacity: 0, duration: 0.3, onComplete: () => showLoader.value = false })

})

onUnmounted(() => clearInterval(exhaustInterval))
</script>

<style scoped>
.preloader {
  position: fixed !important;
  inset: 0;
  background: #05050a;
  z-index: 2000;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-text {
  position: absolute;
  font-family: monospace;
  font-size: 2.1rem;
  color: #ffffff;
  letter-spacing: 0.05em;
  font-weight: bold;
}
.rocket {
  position: absolute;
  bottom: 10%;
  left: 50%;
  width: 50px;
  transform: translateX(-50%);
  z-index: 2;
}

.flame {
  animation: flicker 0.15s infinite alternate;
  transform-origin: top center;
}

@keyframes flicker {
  from { transform: scaleY(1); }
  to   { transform: scaleY(1.3); }
}

.exhaust-wrap {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 1;
}

.exhaust-particle {
  position: fixed;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: radial-gradient(circle, #fbbf24, transparent 70%);
}

</style>
