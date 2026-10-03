<template>
  <nav class="custom-navbar" ref="navRef">
    <a href="#" class="logo">
      <span class="logo-icon">&lt;<span class="logo-text">Aman</span>/&gt;</span>
    </a>

    <button class="navbar-toggler" @click="isOpen = true">
      <span class="navbar-toggler-icon d-flex align-items-center">☰</span>
    </button>

    <ul class="nav-links desktop-only">
      <li><a href="#Home">Home</a></li>
      <li><a href="#About">About</a></li>
      
      <li><a href="#Skills">Skills</a></li>
      <li><a href="#Projects">Projects</a></li>
      <li><a href="#Contact">Contact</a></li>
    </ul>
  </nav>

  <div class="overlay" :class="{ open: isOpen }" @click="isOpen = false"></div>

  <aside class="sidebar" :class="{ open: isOpen }">
    <button class="close-btn" @click="isOpen = false">✕</button>
    <ul class="sidebar-links">
      <li><a href="#Home" @click="isOpen = false">Home</a></li>
      <li><a href="#About" @click="isOpen = false">About</a></li>
      <li><a href="#Education" @click="isOpen = false">Education</a></li>
      <li><a href="#Skills" @click="isOpen = false">Skills</a></li>
      <li><a href="#Projects" @click="isOpen = false">Projects</a></li>
      <li><a href="#Contact" @click="isOpen = false">Contact</a></li>
    </ul>
  </aside>
</template>

<script setup>
import { ref } from 'vue'

const isOpen = ref(false)
const navRef  = ref(null);
defineExpose({ navRef })
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@600;700&display=swap');
.custom-navbar {
  position: fixed !important;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: min(90%, 1100px);
  margin: 1.5rem 0 1.5rem 0;
  display: flex !important;
  align-items: center;
  justify-content: space-between;
  padding: 0.8rem 2rem;
  background: rgba(255, 255, 255, 0.06);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 50px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  z-index: 1000;
}

.logo {
  text-decoration: none;
  font-family: 'Sora', sans-serif;

}

/* Subtle coding bracket accent */
.logo-icon {
  color: #ffffff;
  font-weight: 700;
  font-size: 1.1rem;
}

/* Your animated gradient name */
.logo-text {
  font-weight: 700;
  font-size: 1.25rem;
background: linear-gradient(
    130deg,
    #4d4c4c 0%,
    #747272 35%,
    #ffffff 45%,
    #ffffff 55%,
    #676464 65%,
    #7a7979 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  background-size: 250% 100%;
   color: transparent;
     /* filter: drop-shadow(0 0 8px rgba(255, 255, 255, 0.5)); */

  animation: moveGradient 5s linear infinite;
}

@keyframes moveGradient {
 from { background-position: 100% 50%; }
  to   { background-position: 0% 50%; }
}

/* ---------- toggle button (mobile) ---------- */

.navbar-toggler {
  display: block !important;
  border: none;
  background: transparent;
  cursor: pointer;
  font-size: 1.5rem;
  color: rgb(119, 138, 138);
  line-height: 1;
}

/* ---------- desktop menu (hidden on mobile by default) ---------- */

.desktop-only {
  display: none !important;
  gap: 1.8rem;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.nav-links a {

  font-weight: 700;
  display: inline-block;
  color: #d5dbe1;
  text-decoration: none;
  transform: translateY(0);
  transition: transform 0.3s ease;
  font-size: 1rem;

}
.nav-links a:hover {
  color: #fcfcfc;
  font-weight: 700;
  transform: translateY(-5px);

}

/* ---------- breakpoint: switch to desktop layout ---------- */

@media (min-width: 992px) {
  .desktop-only {
    display: flex !important;
  }
  .navbar-toggler {
    display: none !important;
  }
}

/* ---------- overlay ---------- */
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(19, 16, 16, 0.5);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  z-index: 1001;
}
.overlay.open {
  opacity: 1;
  pointer-events: auto;
}

/* ---------- sliding sidebar ---------- */
.sidebar {
  position: fixed;
  top: 0;
  right: 0;
  height: 100vh;
  width: min(75vw, 260px);
  background: rgba(13, 13, 24, 0.97);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  border-left: 1px solid rgba(255, 255, 255, 0.1);
  padding: 2rem 1.5rem;

  transform: translateX(100%);
  transition: transform 0.35s ease;
  z-index: 1002;
}
.sidebar.open {
  transform: translateX(0);
}

.close-btn {
  background: transparent;
  border: none;
  color: white;
  font-size: 1.5rem;
  margin-bottom: 2rem;
  cursor: pointer;
}

.sidebar-links {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  list-style: none;
  padding: 0;
}
.sidebar-links a {

  font-weight: 700;
  display: inline-block;
  color: #bdc0c4;
  text-decoration: none;
  transform: translateY(0);
  transition: transform 0.3s ease;
}
.sidebar-links a:hover {
  color: #fbfbfb;
  font-weight: 700;
  transform: translateY(-5px);
}

@media (max-width: 480px) {
  .custom-navbar {
    padding: 0.6rem 1rem;
    width:  94%;
    margin-top: 0.8rem;
  }
  .logo-text {
    font-size: 1rem;
  }
}
</style>
