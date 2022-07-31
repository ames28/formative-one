<template>
  <div id="appContainer">
    <button @click="newsButton()" class="pointer" :class="{active: newsFeedClick == true }">News Feed</button>
    <button @click="aboutButton()" class="pointer" :class="{active: aboutPageClick == true }">About Page</button>
    <NewsApp :apiKey="apiKey" v-show="newsFeedClick" />
    <AboutPage v-show="aboutPageClick" />
  </div>
</template>

<script>
import NewsApp from './components/News.vue'
import AboutPage from './components/About-Page.vue'

export default {
  name: 'app',
  data: () => {
    return {
      apiKey: 'aa920a66e07a42cd96bf10ac5424887c',
      aboutPageClick: false,
      newsFeedClick: true
    }
  },
  components: {
    NewsApp,
    AboutPage,
},
  methods: {
    aboutButton(){
      this.aboutPageClick = true;
      this.newsFeedClick = false;
    },
    newsButton(){
      this.aboutPageClick = false;
      this.newsFeedClick = true;
    }
  }
}

</script>

<style>

@import url('https://use.fontawesome.com/releases/v5.7.2/css/all.css');
  
  #appContainer {
   background-color: blanchedalmond;
   width: 90%;
   margin: 0 auto;
  }

  .pointer {cursor: pointer;}
  
  button {
    border: none;
    background-color: blanchedalmond;
    color: black;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
  }
  .active {
    color: blueviolet;
    border-bottom: 1px solid blueviolet;
  }

  
  
</style>
