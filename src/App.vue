<template>
  <div id="app">
    <VueForm />
  </div>
</template>

<script>
import VueForm from './components/VueForm.vue'

export default {
  name: 'App',
  components: {
    VueForm
  }
}
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 100%;
}
</style>
