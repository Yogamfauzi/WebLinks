<template>
  <Home msg="Web Links" />
</template>

<script setup>
  import Home from './components/Home.vue';
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
.flex {
    display: flex; /* Enable flexbox */
    align-items: center; /* Vertically center items */
}
</style>
