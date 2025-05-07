<script setup>
import Navbar from './components/Navbar.vue'
import Introduction from './components/Introduction.vue'
import Requirement from './components/Requirements.vue'
import Installation from './components/Installation.vue'
</script>

<template>
  <header class="sticky-top">
    <Navbar />
  </header>
  <main class="p-4">
    <img src="./assets/arch.png" alt="header-background" class="img-fluid rounded" />
    <section class="d-flex flex-column">
      <h1 class="display-3 text-center">Arch Linux + Hyprland installation</h1>
    </section>
    <hr />
    <section>
      <article>
        <Introduction />
      </article>
    </section>

    <section>
      <h2 class="text-center">Installation Process</h2>
      <h3>Pre-requisites</h3>
      <article>
        <Requirement />
      </article>
      <article class="mt-5">
        <Installation />
      </article>
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
