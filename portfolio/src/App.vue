<script setup>
import AboutMe from './components/AboutMe.vue';

import ContactSection from './components/ContactSection.vue';
import FooterSection from './components/FooterSection.vue';
import HeroBackground from './components/HeroBackground.vue';
import ProjectSection from './components/ProjectSection.vue';
import SkillSection from './components/SkillSection.vue';
import starField from './components/starField.vue';

</script>

<template>
  <starField   />
   <main>

<HeroBackground/>
  <AboutMe/>
  <SkillSection />
  <ProjectSection />
  <ContactSection />
  <FooterSection />

   </main>

</template>
