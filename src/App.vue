<script setup>
import Navbar from './components/Navbar.vue'
import Introduction from './components/Introduction.vue'
import Installation from './components/Installation.vue'
</script>

<template>
  <header>
    <Navbar />
    <img src="./assets/arch.png" alt="header-background" class="img-fluid" />
  </header>
  <main class="p-4">
    <section class="d-flex flex-column">
      <h1 class="display-3 text-center">Arch Linux + Hyprland installation</h1>
    </section>
    <hr />
    <article>
      <Introduction />
    </article>

    <section>
      <Installation />
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
</style>
