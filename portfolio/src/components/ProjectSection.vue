<script setup>

import { ref , onMounted , onUnmounted , nextTick } from 'vue';
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger)
ScrollTrigger.config({ ignoreMobileResize: true })

const refreshScroll = () => {
    ScrollTrigger.refresh();
};
const projects = ref([

{
    project_name : 'Trek Thrill',project_desc:'A full-stack web application tailored for outdoor adventures. Administrators can seamlessly create itineraries, oversee bookings, and assign expert guides to specific treks from a centralized dashboard, while users can easily browse and book solo or group expeditions.' , tools:['html' , 'css' , 'bootstrap' ,'vue.js' , 'flask'] , github_link:'https://github.com/25dp1000037/Trekking_Management_Application_V2'
}
,{
    project_name : 'My Portfolio',project_desc:'A performant, space-themed developer portfolio. It leverages Vue 3, Bootstrap, and GSAP to create an engaging, fully responsive interface that highlights my technical projects and expertise as a full-stack developer.' , tools:['html' , 'css' , 'bootstrap' ,'vue.js' , 'Gsap'] , github_link:'https://github.com/AmanGupta0510/MY-PORTFOLIO'
}


])
let ctx;
onMounted(() =>{


    nextTick(() =>{

        ctx = gsap.context(()=>{

            gsap.set(['.project-text' , '.project-wrapper'],{ y: 40, opacity: 0 })

            gsap.timeline({

                defaults:{ease:'power3.out'},
                scrollTrigger:{
                    trigger:'.project-text',
                    start:'top 85%',
                    toggleActions:'play reverse play reverse',
                    markers:true
                }

            }).to('.project-text', {opacity: 1, y: 0, duration: 0.8,})

            gsap.timeline({
                defaults:{ease:'power3.out'},
                scrollTrigger:{
                    trigger:'.project-outer-container',
                    start:'top 75%',
                    toggleActions:'play reverse play reverse',
                    markers:true
                }
            }).to('.project-wrapper' , {opacity:1 , y:0 , duration:0.8 , stagger:0.15})

        })

    })

})

onUnmounted(() =>{
    ctx.revert();
})

</script>

<template>

    <section id="projects" class="project-section">

        <div class="d-flex pb-2 justify-content-center align-items-center">
            <span class="project-text">
                Projects
                <div class="custom-border">
                </div>
            </span>
        </div>



    <div class="project-outer-container">

      <div class="project-wrapper" v-for="(project, index) in projects" :key="index">

        <div class="project-index">{{ String(index + 1).padStart(2, '0') }}</div>

        <div class="project-card">
          <div class="project-img-box">
            <img :src= "project.image" :alt="project.project_name" class="project-img"  @load="refreshScroll"/>
          </div>

          <div class="project-details">
            <h4 class="project-name">{{ project.project_name }}</h4>
            <p class="project-desc">{{ project.project_desc }}</p>

            <div class="project-tools">
              <span class="tool-tag" v-for="(tool, i) in project.tools" :key="i">
                {{ tool }}
              </span>
            </div>

            <a :href="project.github_link" target="_blank" rel="noopener" class="project-link">
              <i class="fa fa-github"></i> View on GitHub
            </a>
          </div>
        </div>

      </div>

    </div>


    </section>




</template>


<style scoped>


.project-section {
  position: relative;
  z-index: 1;
  padding: 5rem 1.5rem 3rem;
  scroll-margin-top: 90px;
}

.project-text {
  font-size: clamp(2.5rem, 4vw, 2.8rem);
  font-family: monospace;
  font-weight: 800;
  color: #ffffff;
  text-shadow: 0 0 12px rgba(255, 255, 255, 0.6);
}

.custom-border {
  width: 70%;
  height: 3px;
  margin: 0 auto;
  background: linear-gradient(90deg, #b224ef, #7579ff);
  border-radius: 20px;
}

/* ---------- the scrollable container ---------- */
.project-outer-container {
  max-width: 900px;
  margin: 2rem auto 0;
  max-height: 65vh;          /* fixed height cap: this is what creates the inner scroll */
  overflow-y: auto;          /* scrolls vertically inside the box, not the whole page */
  overflow-x: hidden;
  padding: 0.5rem 1rem 0.5rem 0.5rem;   /* right padding leaves room for the scrollbar */
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

/* thin, themed scrollbar (Chrome/Edge/Safari) */
.project-outer-container::-webkit-scrollbar {
 display: none;
}

/* ---------- one project row ---------- */
.project-wrapper {
  display: flex;
  align-items: flex-start;
  gap: 1.5rem;
}

.project-index {
  font-family: monospace;
  font-size: clamp(1.8rem, 3vw, 2.5rem);
  font-weight: 800;
  line-height: 1;
  background: linear-gradient(135deg, #4ed591, #d0d0d8);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  min-width: 3rem;
  padding-top: 0.5rem;
}

.project-card {
  flex: 1;
  display: flex;
  gap: 1.5rem;
  padding: 1.2rem;
  background: rgba(255, 255, 255, 0.04);
  backdrop-filter: blur(18px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.project-card:hover {
  transform: translateY(-4px);
  border-color: #20c997;
}

/* ---------- image ---------- */
.project-img-box {
  flex: 0 0 38%;

  aspect-ratio: 16 / 10;
  border-radius: 8px;
  overflow: hidden;
}

.project-img {
  width: 100%;
  height: 100%;
  object-fit: cover;

}

/* ---------- details ---------- */
.project-details {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;

}

.project-name {
  margin: 0;
  font-size: 1.2rem;
  color: #ffffff;
}

.project-desc {

  margin: 0;
  font-size: 0.9rem;
  color: #a0a8b3;
  line-height: 1.5;
}

.project-tools {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.tool-tag {
  text-transform: uppercase;
  padding: 0.2rem 0.7rem;
  font-size: 0.75rem;
  color: #cbd5e1;
  background: rgba(117, 121, 255, 0.15);
  border: 1px solid rgba(117, 121, 255, 0.35);
  border-radius: 20px;
}

.project-link {
  align-self: flex-start;
  margin-top: auto;
  font-size: 0.85rem;
  color: #20c997;
  text-decoration: none;
}
.project-link:hover {
  text-decoration: underline;
}

/* ---------- mobile ---------- */
@media (max-width: 768px) {
  .project-outer-container {
    max-height: 70vh;
    max-width: 500px;
     padding:3rem 0rem
  }
  .project-wrapper {
    flex-direction: column;
    gap: 0.5rem;
  }
  .project-card {
    flex-direction: column;
  }
  .project-img-box {
    flex: none;
    width: 100%;
  }
}
</style>
