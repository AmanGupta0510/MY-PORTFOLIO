<script setup>

import { onMounted , onUnmounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);
ScrollTrigger.config({ ignoreMobileResize: true });

let ctx;

onMounted(() => {


    ctx = gsap.context(() =>{

        gsap.set(
            ['.about'  , '.about-text','.bio' ] , {y:20}
        );
        gsap.set(['.stat-card-wrap'] , {y:20 , opacity:0});

        const tl = gsap.timeline({
            defaults:{ease:'power3.out'},
            scrollTrigger:{
                trigger:'.about',
                start:'top 70%',
                toggleActions:'play reverse play reverse',
                // markers:true,
            }
        })
        tl.to('.about  .about-text ' , {opacity:1 , y:0 , duration:0.6})
          .to('.bio' , {opacity:1 , y:0 , duration:0.7 } , '-=0.4')



        const mm = gsap.matchMedia()

        mm.add('(min-width:768px)' , ()=>{
            gsap.timeline({
                defaults:{ease:'power3.out'},
                scrollTrigger:{
                    trigger:'.stat-card-wrap',
                    start:'top 80%',
                    toggleActions:'play reverse play reverse'
                }
            }).to('.stat-card-wrap' , {opacity:1  , y:0, duration:0.5, stagger:{each:0.09 , from:'center'}, clearProps:'transform'} )
        })

        mm.add('(max-width:767px)' , ()=>{
            gsap.timeline({
                defaults:{ease:'power3.out'},
                scrollTrigger:{
                    trigger:'.stat-card-wrap',
                    start:'top 90%',
                    end:'bottom 30%',
                    scrub:1,
                    markers:'true'
                }
            }).to('.stat-card-wrap' , {opacity:1  , y:0, duration:0.5, stagger:0.5 , ease:'back.out(1.4)'})
        })




    })



})

onUnmounted(() =>{
    ctx.revert()
})

</script>

<template>
    <section id="#About" class="about   p-md-5">

        <div class="d-flex pb-2 justify-content-center align-items-center">
            <span class="about-text">
                About Me
                <div class="custom-border">
                </div>
            </span>
        </div>


        <div class="container d-flex justify-content-center">



                <p class="bio">
                    I'm Aman Gupta, a Software Engineer and a Web Developer and first-year MCA student based in Bihar. I am passionate about full-stack development, continuous learning, and turning great ideas into functional, user-friendly websites.
                    <span  class="desktop-only"  style="color: #a0a8b3; font-family: monospace; font-size: clamp(1rem , 4vw , 1.2rem); font-weight: bold;">
                         I enjoy bridging the gap between clean backend logic and highly engaging, responsive front-end designs. Currently, my focus is on mastering modern web technologies, building scalable applications, and tackling complex coding challenges. I believe that great web development is not just about writing code, but about crafting digital experiences that are intuitive, efficient, and visually striking.
                    </span>
                </p>


        </div>
    <div class="stats-grid container">

        <div class="stat-card-wrap">
            <div class="stat-card ">
            <h3 class="stat-number">300+</h3>
            <h4 class="stat-title">LeetCode Solved</h4>
            <p class="stat-desc">Practicing Data Structures & Algorithms in Java.</p>
        </div>
        </div>


        <div class="stat-card-wrap">

            <div class="stat-card ">
            <h3 class="stat-number">1+</h3>
            <h4 class="stat-title">Years Experience</h4>
            <p class="stat-desc">Continuous learning in full-stack web development.</p>
        </div>
        </div>




        <div class="stat-card-wrap">
             <div class="stat-card ">
            <h3 class="stat-number">Core</h3>
            <h4 class="stat-title">Tech Stack</h4>
            <p class="stat-desc">Python, Flask, Vue.js, and relational databases.</p>
        </div>

        </div>


        <div class="stat-card-wrap">
 <div class="stat-card ">
            <h3 class="stat-number">System</h3>
            <h4 class="stat-title">Architecture</h4>
            <p class="stat-desc">Building APIs and handling background jobs.</p>
        </div>
        </div>


    </div>





    </section>

</template>

<style scoped>



.about{

    position: relative;
    z-index: 1;
    min-height: 100vh;
}

.about-text{
    opacity: 0;
    font-size:clamp(2.5rem , 4vw , 3.3rem);
    font-family: monospace;
    font-weight: 800;
    color: #ffffff;
    text-shadow: 0 0 12px rgba(255, 255, 255, 0.6);
}
.custom-border{

    width: 70%;
    height: 3px;
    margin: 0 auto;
    background: linear-gradient(90deg, #b224ef, #7579ff);
    border-radius: 20px;
}
.bio{
  opacity: 0;
  font-family: monospace;
  font-size: clamp(1rem , 4vw , 1.2rem);
  color: #a0a8b3;
  line-height: 2rem;
  padding: 2rem;
  font-weight:bold;
  margin: 0;
}
.stats-grid{

    display: grid;
    grid-template-columns: repeat(auto-fit ,  minmax(200px , 1fr));
    gap: 18px;
}
.stat-card{

  background: rgba(255, 255, 255, 0.03); /* Very dark, transparent white */
  border: 1px solid rgba(255, 255, 255, 0.1); /* Subtle initial border */
  border-radius: 12px;
  padding: 24px 16px;
  transform: translateY(0);
  transition: transform 0.3s ease;
  backdrop-filter: blur(8px);


}
.stat-number{
  font-size: 2.2rem;
  color: #ffffff;
  margin: 0px 0px 8px 0px;
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}
.stat-title {
  font-size: 1.1rem;
  margin: 0px 0px 8px 0px;
  color: #20c997;
  letter-spacing: 1px;
}
.stat-desc {
  font-size: 0.9rem;
  color: #999999;
  margin: 0;
  line-height: 1.4;
}

.stat-card:hover{

   transform:   translateY(-5px);
  border-color: #20c997;
  box-shadow: 0 8px 24px rgba(32, 201, 151, 0.15);
}
@media (max-width: 768px){
    .desktop-only{
        display: none;
    }
    .bio{
        text-align: center;
        line-height: 2rem;
    }
}

@media (max-width:600px){

    .stats-grid{
        grid-template-columns: 1fr;
    }
}
</style>
