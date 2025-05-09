<script setup>
import Navbar from './components/Navbar.vue'
import Introduction from './components/Introduction.vue'
import Requirement from './components/Requirements.vue'
import Installation from './components/Installation.vue'
import { ref, onMounted, onUnmounted } from 'vue'

const sections = ['introduction', 'installation', 'about-me']
const activeSection = ref(null)

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          activeSection.value = entry.target.id
        }
      })
    },
    {
      root: null,
      rootMargin: '0px',
      threshold: 0.01,
    },
  )

  const sectionElements = document.querySelectorAll('section[id]')
  sectionElements.forEach((el) => observer.observe(el))

  onUnmounted(() => {
    sectionElements.forEach((el) => observer.unobserve(el))
  })
})
</script>

<template>
  <header class="sticky-top">
    <Navbar :sections="sections" :active-section="activeSection" />
  </header>
  <main class="p-4" data-bs-spy="scrol" data-bs-target="">
    <img src="./assets/arch.png" alt="header-background" class="img-fluid rounded" />
    <section class="d-flex flex-column">
      <h1 class="display-3 text-center">Arch Linux + Hyprland installation</h1>
    </section>
    <hr />
    <section id="introduction" class="mb-5">
      <article>
        <Introduction />
      </article>
    </section>
    <section id="installation" class="mt-5">
      <h2 class="text-center">Installation Process</h2>
      <h3>Pre-requisites</h3>
      <article>
        <Requirement />
      </article>
      <article class="mt-5">
        <Installation />
      </article>
    </section>
    <hr />
    <section id="about-me">
      <article>About me</article>
    </section>
  </main>
</template>

<style scoped>
.img-fluid {
  width: 100%;
  max-height: 60vh;
  height: auto;
  object-fit: cover;
}

section {
  padding-left: 40px;
  padding-right: 40px;
}
</style>
