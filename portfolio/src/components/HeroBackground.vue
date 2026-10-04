<!-- HeroBackground.vue -->
<script setup>



import { ref, watch, nextTick, onUnmounted } from 'vue'

import gsap from 'gsap'

import Preloader from './PreLoader.vue'

import NavBar from './NavBar.vue'

const pageReady = ref(false)
let messageIndex = 0
let roleInterval = null

const role = ['Full Stack Developer', 'Frontend Developer', 'Backend Developer']
const roleMessage = ref(role[0])
const navElem = ref(null)


watch(pageReady, async (val) => {
  if (!val) return
  await nextTick()




  roleInterval = setInterval(() => {
    gsap.to('.role' , {
      opacity:0,
      y:-10,
      duration:0.6,
      onComplete:()=>{
        messageIndex = (messageIndex + 1) % role.length
        roleMessage.value = role[messageIndex]
        gsap.fromTo('.role' , {opacity:0,y:10} , {opacity:1 , y:0 , duration:0.6})
      },

    })

  }, 1800)


  const tl = gsap.timeline({defaults:{ease:'power3.out'}});
  gsap.set(['eyebrow' , '.name' , '.role-container' , '.desc-desktop' , '.desc-mobile'] , {x:-20 , opacity:0})
  gsap.set('.social-icon' , {y:20})
  gsap.set('.hero-photo', {scale:0.75 , rotate:-9})
  gsap.set(navElem.value.navRef , {y:-20});
  tl.to(navElem.value.navRef , {opacity:1 , y:0 , duration:0.8 , clearProps:'transform'})
    .to('.eyebrow' , {opacity:1 , x:0 , duration:0.6} , '-=0.4')
    .to('.name'  , {opacity:1 , x:0 , duration:0.7} , '-=0.35' )
    .to('.role-container' , {opacity:1 , x:0 , duration:0.7} , '-=0.35')
    .to('.desc-desktop' , {opacity:1 , x:0 , duration:0.7} , '-=0.35')
    .to('.social-icon' , {opacity:1 , y:0 , duration:0.5, stagger:{each:0.09 , from:'center'}, clearProps:'transform', ease:'back.out(1.7)' } , '-=0.3')
    .to('.hero-photo' , {opacity:1 , scale:1 , rotate:0 , duration:0.9 , ease:'back.out(1.6)'} , '-=0.6')



    const mm = gsap.matchMedia();

    mm.add('(max-width:372px)', () => {
      gsap.timeline({
       defaults:{ease:'power3.out'}
      })

       .to(navElem.value.navRef , {opacity:1 , y:0 , duration:0.8 , clearProps:'transform'})
    .to('.eyebrow' , {opacity:1 , x:0 , duration:0.6} , '-=0.4')
    .to('.name'  , {opacity:1 , x:0 , duration:0.7} , '-=0.35' )
    .to('.role-container' , {opacity:1 , x:0 , duration:0.7} , '-=0.35')
     .to('.desc-mobile', {  opacity: 1, x: 0, duration: 0.7} , '-=0.35')
    .to('.social-icon' , {opacity:1 , y:0 , duration:0.5, stagger:{each:0.09 , from:'center'}, clearProps:'transform', ease:'back.out(1.7)' } , '-=0.3')
    .to('.hero-photo' , {opacity:1 , scale:1 , rotate:0 , duration:0.9 , ease:'back.out(1.6)'} , '-=0.6')

    })
})


onUnmounted(() => {
  clearInterval(roleInterval)
})
</script>

<template>
  <Preloader @done="pageReady = true" />
  <NavBar v-if="pageReady" ref="navElem" />

  <section class="hero  min-vh-100 d-flex align-items-center scroll-section" id="home">
    <div class="container">
      <div class="row justify-content-center align-items-center">
        <div class="col-12 col-lg-6 px-3 px-md-5 order-2 order-lg-1">
          <div class="hero-text text-center text-lg-start" ref="heroText">
            <p class="eyebrow d-flex justify-content-center justify-content-lg-start">Hi , I'm</p>
            <h1 class="name">Aman Gupta</h1>
            <div class="role-container">
                   <p class="role">{{ roleMessage }}</p>
            </div>
            <div class="desc-container">
              <p class="desc-mobile">
                Building interactive, high-performance web applications seamlessly across the frontend and backend.
              </p>
              <p class="desc-desktop">
                Full-stack developer building high-performance, interactive websites. Working in
                Frontend, Backend to turn ideas into clean efficient user-friendly web applications.
              </p>
            </div>
          </div>

          <div class="social-links-outer-box">
            <div class="social-links">
              <a
                href="https://github.com/amangupta0510"
                target="_blank"
                rel="noopener"
                class="social-icon github"
                aria-label="GitHub"
              >
                <i class="fa fa-github"></i>
              </a>

              <a
                href="https://instagram.com/amngupta510"
                target="_blank"
                rel="noopener"
                class="social-icon instagram"
                aria-label="Instagram"
              >
                <i class="fa fa-instagram"></i>
              </a>

              <a
                href="https://linkedin.com/in/your-username"
                target="_blank"
                rel="noopener"
                class="social-icon linkedin"
                aria-label="LinkedIn"
              >
                <i class="fa fa-linkedin-square"></i>
              </a>

              <a
                href="https://leetcode.com/OptimisticAmn"
                target="_blank"
                rel="noopener"
                class="social-icon leetcode"
                aria-label="LeetCode"
              >
                <svg viewBox="0 0 24 24" fill="currentColor">
                  <path
                    d="M13.483 0a1.374 1.374 0 0 0-.961.438L7.116 6.226l-3.854 4.126a5.266 5.266 0 0 0-1.209 2.104 5.35 5.35 0 0 0-.125.513 5.527 5.527 0 0 0 .062 2.362 5.83 5.83 0 0 0 .349 1.017 5.938 5.938 0 0 0 1.271 1.818l4.277 4.193.039.038c2.248 2.165 5.852 2.133 8.063-.074l2.396-2.392c.54-.54.54-1.414.003-1.955a1.378 1.378 0 0 0-1.951-.003l-2.396 2.392a3.021 3.021 0 0 1-4.205.038l-.02-.019-4.276-4.193c-.652-.64-.972-1.469-.948-2.263a2.68 2.68 0 0 1 .066-.523 2.545 2.545 0 0 1 .619-1.164L9.13 8.114c1.058-1.134 3.204-1.27 4.43-.278l3.501 2.831c.593.48 1.461.387 1.94-.207a1.384 1.384 0 0 0-.207-1.943l-3.5-2.831c-.8-.647-1.766-1.045-2.774-1.202l2.015-2.158A1.384 1.384 0 0 0 13.483 0zm-2.866 12.815a1.38 1.38 0 0 0-1.38 1.382 1.38 1.38 0 0 0 1.38 1.382H20.79a1.38 1.38 0 0 0 1.38-1.382 1.38 1.38 0 0 0-1.38-1.382z"
                  />
                </svg>
              </a>
            </div>
          </div>
        </div>

        <div class="col-12 col-lg-6 d-flex justify-content-center order-1 order-lg-2 mb-4 mb-lg-0">
          <div class="photo-frame">
            <img src="/photo4.jpeg" alt="Aman Gupta" class="hero-photo" />
          </div>
        </div>
      </div>
    </div>
  </section>


</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@800&display=swap');

.hero {
  position: relative;
  z-index: 1;

  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 3rem;
   padding: 5rem 0;
  font-family: 'Syne', sans-serif;
}

.hero-text {
  max-width: 800px;
}
.desc-desktop{
  opacity: 0;
  font-family: monospace;
  font-size: 1rem;
  color: #a0a8b3;
  line-height: 1.6;
  max-width: 480px;
  font-weight: bold;
}

.eyebrow {
  opacity: 0;
  font-size: 1.3rem;
  font-weight: bold;
  color: #818283c2;
  margin-bottom: 0.5rem;
}

.name {
   opacity: 0;
  font-family: 'Syne', sans-serif;
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  font-weight: 800;
  line-height: 1.1;
  margin: 0 0 0.5rem;
  color: #ffffff;
  text-shadow:
    0 0 10px rgba(255, 255, 255, 0.3),
    0 0 20px rgba(56, 189, 248, 0.4);
}
.role-container{
   opacity: 0;
}

.role {
  font-size: clamp(1.2rem, 2vw, 2.5rem);
  font-family: monospace;
  background: linear-gradient(135deg, #10b981 0%, #6ee7b7 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: bold;
  filter: drop-shadow(0 0 12px rgba(56, 248, 98, 0.5));
  margin-bottom: 1rem;
}

.desc-mobile {
  display: none;
  opacity: 0;
  font-family: monospace;
  font-size: 1rem;
  color: #a0a8b3;
  line-height: 1.6;
  max-width: 480px;
  font-weight: bold;
}

.hero-photo {
   opacity: 0;
  width: 21rem;
  height: 21rem;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid rgba(198, 202, 204, 0.4);
  box-shadow: 0 0 40px rgba(7, 23, 80, 0.504);
}

.social-links {

  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: center;
  gap: clamp(14px, 6vw, 90px);
}

.social-icon {
  /* scales from 32px on phones up to 48px on desktop */
  opacity: 0;
  font-size: clamp(32px, 7vw, 40px);
  line-height: 1;
  display: inline-flex;
  text-decoration: none;
  transition:
    transform 0.2s ease,
    filter 0.2s ease;
}

.social-icon svg {
  width: 1em; /* matches the font icons' size */
  height: 1em;
}

.social-icon:hover {
  transform: translateY(-4px) scale(1.1);
}

/* original brand colors */
.github {
  color: #b5b3b3;
}
.linkedin {
  color: #0a66c2;
}
.leetcode {
  color: #ffa116;
}
.instagram i {
  background: linear-gradient(45deg, #feda75, #fa7e1e, #d62976, #962fbf, #4f5bd5);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

@media (max-width: 991px) {
  .desc-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .social-links-outer-box {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .hero-photo {
    margin-top: 1.5rem;
    width: 15rem !important;
    height: 15rem !important;
  }
}
@media (max-width: 375px) {
  .role {
    font-size: clamp(1.3rem, 3vw, 2.5rem);
  }
  .hero-photo {
    width: 13rem;
    height: 13rem;
  }
  .photo-frame {
    margin-top: 2.5rem;
  }
  .desc-desktop{
    display: none;
  }
  .desc-mobile{
    display: block;
  }
}
</style>

