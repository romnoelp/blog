<script setup>
const props = defineProps(['sections', 'activeSection'])
</script>

<template>
  <nav class="navbar bg-body-tertiary">
    <div class="container-fluid">
      <div>
        <a class="navbar-brand" href="#">
          <img
            src="./icons/archlinux.svg"
            alt="Logo"
            width="30"
            height="24"
            class="d-inline-block align-text-top"
          />
          ArchHyde
        </a>
      </div>
      <div class="nav">
        <ul class="nav justify-content-end" v-for="section in props.sections" key="index">
          <li class="nav-item">
            <a
              :href="'#' + section"
              class="nav-link"
              :class="{ active: props.activeSection === section }"
            >
              {{ section }}
            </a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<style scoped>
ul a {
  color: #1887bf;
}

.nav-link.active {
  font-weight: bold;
  color: #005f99;
}
</style>
