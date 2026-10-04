<script setup>
import { onMounted , onUnmounted, ref } from 'vue';
const currentYear = new Date().getFullYear();

const emojisArr = ['❤️' , '☕' , '🥰'];


const emojis = ref(emojisArr[0]);
 let indx = 0;
let footer_emojis_interval ;
onMounted(() =>{

    footer_emojis_interval = setInterval(() =>{

        emojis.value= emojisArr[(indx) % emojisArr.length]
        indx++;
    } , 1300)
})

onUnmounted(() =>{
    indx = 0;
    clearInterval(footer_emojis_interval);
})
</script>

<template>
    <section id="footer" class="footer-section">
        <div class="footer-content">
            <div class="footer-left">
                <p>&copy; {{ currentYear }} Aman Gupta.All rights reserved.</p>


            </div>
            <p>Made with {{ emojis }} by Aman Gupta 😉</p>
            <div class="footer-right">

                <a href="https://github.com/amangupta0510" target="_blank" rel="noopener noreferrer" class="social-link">GitHub</a>
                <a href="https://linkedin.com/in/aman-gupta-a70bb2361" target="_blank" rel="noopener noreferrer" class="social-link">LinkedIn</a>
                <a href="https://instagram.com/amngupta510" target="_blank" rel="noopener noreferrer" class="social-link">Instagram</a>
                <a href="https://leetcode.com/OptimisticAmn" target="_blank" rel="noopener noreferrer" class="social-link">LeetCode</a>
            </div>
        </div>
    </section>
</template>

<style scoped>
.footer-section {
    position: relative;
    z-index: 1;

    background: transparent;
    backdrop-filter: blur(15px);
    min-height: 10vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    border-top: 1px solid rgba(255, 255, 255, 0.148); /* Subtle top border for separation */
    color: #d2d8e2;
}

.footer-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    max-width: 1200px;
    flex-wrap: wrap;
    gap: 2rem;
}

p {
    margin: 0;
    font-size: 1.1rem;
    letter-spacing: 0.05em;
}

.footer-right {
    display: flex;
    gap: 1.5rem;
}

.social-link {
    color: #b4b8c0;
    text-decoration: none;
    font-size: 1.1rem;
    font-weight: 500;
    transition: color 0.3s ease, transform 0.3s ease;
}

.social-link:hover {
    color: #ffffff;
    transform: translateY(-2px);
}




@media (max-width: 768px) {
    .footer-content {
        flex-direction: column;
        justify-content: space-evenly;
        text-align: center;
    }

    .footer-right {
        margin-top: 0.5rem;
        gap: 1.25rem;
    }
}
</style>
