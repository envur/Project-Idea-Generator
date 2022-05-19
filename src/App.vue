<template>
  <IdeasGenerator />
</template>

<script>
import IdeasGenerator from './components/Generator.vue'

export default {
  name: 'App',
  components: {
    IdeasGenerator
  }
}
</script>

<style lang="scss">
html, body {
  margin: 0;
  height: 100%;
  background: #F9F9F9;
}
.container {
  padding-top: 20px;
}
</style>
