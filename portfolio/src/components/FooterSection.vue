<script setup>
import { onMounted , onUnmounted, ref } from 'vue';
const currentYear = new Date().getFullYear();

const emojisArr = ['❤️' , '☕' , '🥰'];
let indx = 0;

const emojis = ref(emojisArr[indx]);

onMounted(() =>{
    setInterval(() =>{
        indx++;
        emojis .value= emojisArr[(indx) % emojisArr.length]
    } , 1700)
})

onUnmounted(() =>{
    indx=0;
    clearInterval();
})
</script>

<template>
    <section id="footer" class="footer-section">
        <div class="footer-content">
            <div class="footer-left">
                <p>&copy; {{ currentYear }} Aman Gupta.</p>


            </div>
            <p>Made with {{ emojis }} by Aman Gupta 😉</p>
            <div class="footer-right">
                <!-- Replace '#' with your actual profile URLs -->
                <a href="https://github.com/amangupta0510" target="_blank" rel="noopener noreferrer" class="social-link">GitHub</a>
                <a href="#" target="_blank" rel="noopener noreferrer" class="social-link">LinkedIn</a>
                <a href="#" target="_blank" rel="noopener noreferrer" class="social-link">Instagram</a>
                <a href="#" target="_blank" rel="noopener noreferrer" class="social-link">LeetCode</a>
            </div>
        </div>
    </section>
</template>

<style scoped>
.footer-section {
    position: relative;
    z-index: 1;
    /* Using a very dark gray/black to match the bottom of your space gradient */
    background-color: #050505;
    min-height: 10vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    border-top: 1px solid rgba(255, 255, 255, 0.05); /* Subtle top border for separation */
    color: #9ca3af; /* Soft gray text */
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
    color: #9ca3af;
    text-decoration: none;
    font-size: 1.1rem;
    font-weight: 500;
    transition: color 0.3s ease, transform 0.3s ease;
}

.social-link:hover {
    color: #ffffff; /* Brightens up on hover */
    transform: translateY(-2px); /* Slight lift effect on hover */
}

/* Responsive adjustment for mobile screens */
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
