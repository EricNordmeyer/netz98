<script setup>
  const props = defineProps({
  homeLink: String
})
</script>

<template>
  <header>
    <h1>
      <a :href="homeLink"><slot name="brand"></slot></a>
      <slot name="description"></slot>
    </h1>
  </header>
</template>

<style scoped>
  header {
    margin-bottom: 24px;
  }

  h1 a{
    color: #2bafe5;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  }
</style>