<template>
  <theHeaders title="Frequently Ask Questions"/>
  <faqList/>
</template>

<script>
import theHeaders from './components/layouts/theHeaders.vue'
import faqList from './components/faqList.vue'

export default {
  name: 'App',
  components: {
    theHeaders,
    faqList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
