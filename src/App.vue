<template>
  <div id="app">
    <NewsApp :apiKey="apiKey" />
  </div>
</template>

<script>
import NewsApp from './components/News.vue'
export default {
  name: 'app',
  data: () => {
    return {
      apiKey: 'aa920a66e07a42cd96bf10ac5424887c'
    }
  },
  components: {
    NewsApp
  }
}
</script>

<style>

@import url('https://use.fontawesome.com/releases/v5.7.2/css/all.css');
  
  #app {
   background-color: blanchedalmond;
  }
  
</style>
