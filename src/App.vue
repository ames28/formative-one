<template>
  <div id="appContainer">
    <div id="mainButtons">
        <button @click="newsButton()" class="pointer mainButton" :class="{active: newsFeedClick == true }">News Feed</button>
        <button @click="aboutButton()" class="pointer mainButton" :class="{active: aboutPageClick == true }">About Page</button>
    </div>
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
@import url('https://fonts.googleapis.com/css2?family=Questrial&display=swap');
  
  #appContainer {
   background-color: white;
   width: 100%;
   margin: 0 auto;
  }

  .pointer {cursor: pointer;}
  
  .mainButton {
    border: none;
    background-color: blueviolet;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
  }
  .active {
    color: white;
    border-bottom: 1px solid white;
  }

  #mainButtons{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    padding: 10px 0;
    background: blueviolet;

  } 

</style>
