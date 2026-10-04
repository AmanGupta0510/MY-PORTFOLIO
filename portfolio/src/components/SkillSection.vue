<script setup>

import { onMounted , onUnmounted , ref } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)
ScrollTrigger.config({ ignoreMobileResize: true })
const frontend  = ref([
  { name: 'HTML', icon: 'devicon-html5-plain colored' },
  { name: 'CSS', icon: 'devicon-css3-plain colored' },
  { name: 'JavaScript', icon: 'devicon-javascript-plain colored' },
  { name: 'Bootstrap', icon: 'devicon-bootstrap-plain colored' },
  { name: 'Vue.js', icon: 'devicon-vuejs-plain colored' }
])
const backend = ref([
  { name: 'Flask', icon: 'devicon-flask-original colored' },
   { name: 'PostgreSQL', icon: 'devicon-postgresql-plain colored' },
  { name: 'MySQL', icon: 'devicon-mysql-plain colored' }
])

const tools = ref([
  { name: 'Linux', icon: 'devicon-linux-plain colored' },
  { name: 'Git', icon: 'devicon-git-plain colored' },
  { name: 'GitHub', icon: 'devicon-github-original colored' },
  { name: 'GSAP', icon: 'si si-greensock', gsap: true }
])
const programming_language = ref([
  { name: 'Python', icon: 'devicon-python-plain colored' },
  { name: 'Java', icon: 'devicon-java-plain colored' }

])

let ctx;

onMounted(()=>{

    ctx = gsap.context(()=>{

        gsap.set(['.skill-card1' , '.frontend' , '.database' , '.skill-card3' , '.skill-card4' , '.tools'] , { x:30, opacity:0})
        gsap.set(['.skill-card2' ,  '.backend' , '.languages' , '.skill-card5'] , { x:-30, opacity:0})
        gsap.set('.skill-text' , {opacity:0 , y:40})

      gsap.timeline({

        defaults:{ease:'power3.out'},
        scrollTrigger:{
          trigger:'.skill-text',
          start: 'top 90%',
          toggleActions:'play reverse play reverse',

        }


      }).to('.skill-text' , {opacity:1 , y:0 , duration:1.8})

        const tl = gsap.timeline({
            defaults:{ease:'power3.out'},
            scrollTrigger: {
                trigger: '.skill-card1',
                start: 'top 95%',
                toggleActions: 'play none none reverse',
            }
        })
        tl.to('.frontend' , {opacity:1 , x:0 , duration:0.3})
        tl.to('.skill-card1' ,{ opacity: 1, x: 0, duration: 0.8 , stagger:0.09 , clearProps:'transform' } , '+=0.1')


        const tl2 = gsap.timeline({
            defaults:{ease:'power3.out'},
            scrollTrigger: {
                trigger: '.skill-card2',
                start: 'top 95%',
                toggleActions: 'play none none reverse',
            }
        })
        tl2.to('.backend' , {opacity:1 , x:0 , duration:0.3})
        tl2.to('.skill-card2' ,{ opacity: 1, x: 0, duration: 0.8 , stagger:0.09 , clearProps:'transform' } , '+=0.1')


        const tl4 = gsap.timeline({
            defaults:{ease:'power3.out'},
            scrollTrigger: {
                trigger: '.skill-card4',
                start: 'top 95%',
                toggleActions: 'play none none reverse',
            }
        })
        tl4.to('.languages' , {opacity:1 , x:0 , duration:0.3})
        tl4.to('.skill-card4' ,{ opacity: 1, x: 0, duration: 0.8 , stagger:0.09 , clearProps:'transform' } , '+=0.1')


        const tl5 = gsap.timeline({
            defaults:{ease:'power3.out'},
            scrollTrigger: {
                trigger: '.skill-card5',
                start: 'top 95%',
                toggleActions: 'play none none reverse',
            }
        })
        tl5.to('.tools' , {opacity:1 , x:0 , duration:0.3})
        tl5.to('.skill-card5' ,{ opacity: 1, x: 0, duration: 0.8 , stagger:0.09 , clearProps:'transform' } , '+=0.1')




    })

})






onUnmounted(() =>{
    ctx.revert();
})



</script>

<template>

<section id="skills" class="skill-section scroll-section">


    <div class="d-flex pb-2 justify-content-center align-items-center">
            <span class="skill-text">
                Skills
                <div class="custom-border">
            </div>
        </span>
    </div>


    <div class=" main-skill-wrapper mt-3" >

        <div class="frontend-skill-wrapper">

           <h3 class="group-title frontend" >Frontend</h3>
            <div class="skill-grid">
                <div class="skill-card1" v-for="skill in frontend" :key="skill.name">
                    <i :class="skill.icon"></i>
                    <span>{{ skill.name }}</span>

                </div>
            </div>

        </div>

        <div class="backend-skill-wrapper">

            <h3 class="group-title backend">Backend</h3>
            <div class="skill-grid">
                <div class="skill-card2" v-for="skill in backend" :key="skill.name">
                    <i :class="skill.icon"></i>
                    <span>{{ skill.name }}</span>

                </div>
            </div>

        </div>



        <div class="programming-skill-wrapper">

           <h3 class="group-title languages" >Programming Languages</h3>
            <div class="skill-grid">
                <div class="skill-card4" v-for="skill in programming_language" :key="skill.name">
                    <i :class="skill.icon"></i>
                    <span>{{ skill.name }}</span>

                </div>
            </div>

        </div>

        <div class="tools-skill-wrapper">

           <h3 class="group-title tools" >Tools</h3>
            <div class="skill-grid">
                <div class="skill-card5" v-for="skill in tools" :key="skill.name">
                    <i :class="skill.icon" v-if="skill.name === 'GSAP'" style="color: #88CE02;" ></i>
                    <i :class="skill.icon" v-else></i>
                    <span>{{ skill.name }}</span>

                </div>
            </div>

        </div>






    </div>


</section>

</template>

<style scoped>
.skill-section {
  position: relative;
  z-index: 1;
  padding: 8rem 2.5rem 3rem;
  scroll-margin-top: 90px;

}

.skill-text {
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

.main-skill-wrapper {
  max-width: 640px;
  margin: 0 auto;
  padding: 0 1.5rem;
}

.frontend-skill-wrapper,
.backend-skill-wrapper,
.database-skill-wrapper,
.programming-skill-wrapper,
.tools-skill-wrapper {
  margin-bottom: 1.2rem;
}

.tools-skill-wrapper {
  margin-bottom: 0;
}

.group-title {
  font-size: 1.5rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 0.7rem;
  text-align: center;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
}
.group-title.frontend {
  background-image: linear-gradient(135deg, #38bdf8, #818cf8);
}
.group-title.backend {
  background-image: linear-gradient(135deg, #34d399, #10b981);
}
.group-title.database {
  background-image: linear-gradient(135deg, #f59e0b, #fbbf24);
}
.group-title.tools {
  background-image: linear-gradient(135deg, #a78bfa, #c4b5fd);
}
.group-title.languages {
  background-image: linear-gradient(135deg, #f472b6, #fb923c);
}

.skill-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
  padding-bottom: 0.5rem;
}

.skill-card1,
.skill-card2,
.skill-card3,
.skill-card4,
.skill-card5 {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem;
  width: 100px;
  background: rgba(255, 255, 255, 0.04);
  backdrop-filter: blur(18px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.skill-card1:hover,
.skill-card2:hover,
.skill-card3:hover,
.skill-card4:hover,
.skill-card5:hover {
  transform: translateY(-4px);
  border-color: rgba(255, 255, 255, 0.25);
}

.skill-card1 i,
.skill-card2 i,
.skill-card3 i,
.skill-card4 i,
.skill-card5 i {
  font-size: 2rem;
}

/* dark-on-dark icons need inverting to stay visible */
.skill-card2 i.devicon-flask-original,
.skill-card5 i.devicon-github-original,
.skill-card5 i.devicon-linux-plain {
  filter: invert(1);
}

.skill-card1 span,
.skill-card2 span,
.skill-card3 span,
.skill-card4 span,
.skill-card5 span {
  font-size: 0.8rem;
  color: #cbd5e1;
  font-weight: 900;
}

@media (max-width: 600px) {
  .skill-section {
    padding-top: 6rem;
  }
  .skill-card1,
  .skill-card2,
  .skill-card3,
  .skill-card4,
  .skill-card5 {
    width: 85px;
    padding: 1rem;
  }
}


</style>


