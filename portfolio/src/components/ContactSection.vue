<template>


  <section class="contact-section d-flex align-items-center" id="contact">
    <div class="container contact-container">
      <div class="row align-items-center">

        <!-- Left Side: Contact Information -->
        <div class="col-lg-5 mb-5 mb-lg-0 contact-info">
          <h2 class="contact-heading">Let's Connect</h2>
          <p class="contact-subtext">
            Whether you have a question, a project idea, or just want to say hi, my inbox is always open.
          </p>
        </div>

        <!-- Right Side: Contact Form -->
        <div class="col-lg-7">
          <div class="form-wrapper p-4 p-md-5">
            <form @submit.prevent="submitForm" class="contact-form">

              <div class="row">
                <div class="col-md-6 mb-4">
                  <input
                    type="text"
                    class="form-control custom-input"
                    v-model="formData.name"
                    placeholder="Your Name"
                    required
                  />
                </div>
                <div class="col-md-6 mb-4">
                  <input
                    type="email"
                    class="form-control custom-input"
                    v-model="formData.email"
                    placeholder="Your Email"
                    required
                  />
                </div>
              </div>

              <div class="mb-4">
                <input
                  type="text"
                  class="form-control custom-input"
                  v-model="formData.subject"
                  placeholder="Subject"
                  required
                />
              </div>

              <div class="mb-4">
                <textarea
                  class="form-control custom-input"
                  v-model="formData.message"
                  rows="5"
                  placeholder="Your Message..."
                  required
                ></textarea>
              </div>

              <button type="submit" class="btn custom-submit-btn w-100">
                Send Message <i class="fa fa-paper-plane ms-2"></i>
              </button>

            </form>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const formData = ref({
  name: '',
  email: '',
  subject: '',
  message: ''
});

let ctx;

const submitForm = () => {
  console.log('Form submitted:', formData.value);
  // Add your Flask API POST request logic here

  // Reset form after submission
  formData.value = { name: '', email: '', subject: '', message: '' };
};

onMounted(() => {
  ctx = gsap.context(() => {

    

  });

});

onUnmounted(() => {
  ctx.revert();
});
</script>

<style scoped>
.contact-section {
    position: relative;
    z-index: 1;
    padding: 5rem 1.5rem 3rem;
    scroll-margin-top: 90px;
    color: #ffffff;
}

.contact-heading {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 1rem;
  letter-spacing: 2px;
}

.contact-subtext {
  color: #a0aab2;
  font-size: 1.1rem;
  line-height: 1.6;
}

.detail-item span {
  font-size: 1.1rem;
  letter-spacing: 1px;
}

/* Glassmorphism/Dark Theme Form Container */
.form-wrapper {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 16px;
  backdrop-filter: blur(10px);
}

/* Overriding Bootstrap Form Controls for Space Theme */
.custom-input {
  background: rgba(0, 0, 0, 0.4);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #ffffff;
  padding: 14px 20px;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.custom-input::placeholder {
  color: #6c757d;
}

/* The neon green glow on focus to match your hero section */
.custom-input:focus {
  background: rgba(0, 0, 0, 0.6);
  border-color: #28a745; /* Bootstrap success green, adjust to match your exact neon green */
  box-shadow: 0 0 10px rgba(40, 167, 69, 0.25);
  color: #ffffff;
  outline: none;
}

/* Custom Submit Button */
.custom-submit-btn {
  background: transparent;
  color: #28a745;
  border: 1px solid #28a745;
  padding: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 2px;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.custom-submit-btn:hover {
  background: #28a745;
  color: #000000;
  box-shadow: 0 0 15px rgba(40, 167, 69, 0.4);
}
</style>
