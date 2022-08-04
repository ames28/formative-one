<template>
    <div class="container">
        <div class="searchbar">
            <div class="search-content">
                <p>Top Headlines from:</p>
                <select @change="selectCountry()">
                    <option value="nz">New Zealand</option>
                    <option value="au">Australia</option>
                    <option value="us">United States</option>
                    <option value="gb">Great Britian</option>
                </select>
                <button @click="filterNews(event)" class="confirmButton">confirm</button>
            </div>
            <div class="search-content">
                <p>Search News:</p>
                <form @submit.prevent="searchNewsFunction">
                    <input type="text" placeholder="search..." v-model="searchword">
                </form>
                <div class="icon-container">
                    <button class="confirmButton" v-if="!isBusy" @click="searchNewsFunction">Search</button>
                    <i v-else class="fas fa-spinner fa-spin"></i>
                    <button class="confirmButton" @click="getTopNews">Reset</button>
                </div>
            </div>   
        </div>
        <div class="article-results">
          <article v-for="(article, index) in articles" :key="index" @click="navTo(article.url)" class="pointer">
                <img v-if="article.urlToImage" :src="article.urlToImage" alt="">
                <img v-else src="../assets/noimage.png">
                <div class="articleText">
                    <h3>{{article.title}}</h3>
                    <h5>{{article.author}} || {{article.source.name}}</h5>
                    <p>{{article.description}}</p>
                <button class="pointer artButton">View Article</button>
                </div>
           </article>
        </div>
        <div ref="infinitescrolltrigger" id="scroll-trigger">
            <i v-if="showLoader" class="fas fa-spinner fa-spin"></i>
        
        </div>
    </div>
</template>

<script>


export default{
    name: 'NewsApp',
    props: ['apiKey'],
    data(){
        return{
            apiUrl: '',
            isBusy: false,
            showLoader: false,
            currentPage: 1,
            totalResults: 0,
            maxPerPage: 10,
            searchword: '',
            articles: [],
            filterCountry: ''
        }
    },
    computed:{
        pageCount(){
            return Math.ceil(this.totalResults/this.maxPerPage);
        }
    },
    methods: {
        navTo(url){
            window.open(url);
        },
        resetData(){
        this.currentPage = 1;
        this.articles = [];
        },
        selectCountry(){
            this.filterCountry = event.target.value;
            console.log("country = " + this.filterCountry);
        },
        searchNewsFunction() {
            if(this.searchword !== '') {
                this.apiUrl = 'https://newsapi.org/v2/everything?q=' + this.searchword + '&pageSize=' + this.maxPerPage + '&apiKey=' + this.apiKey;
                this.isBusy = true;
                this.resetData();
                this.fetchData();
            }
            else{
                this.getTopNews();
            }
        },
        filterNews() {
            this.apiUrl = 'https://newsapi.org/v2/top-headlines?country=' + this.filterCountry + '&pageSize=' + this.maxPerPage + '&apiKey=' + this.apiKey;
            this.isBusy = true;
            this.searchword = '';
            if(this.searchword !== '') {
                this.apiUrl = 'https://newsapi.org/v2/top-headlines?country=' + this.filterCountry + '&q=' + this.searchword + '&pageSize=' + this.maxPerPage + '&apiKey=' + this.apiKey;
                this.isBusy = true;
            }
            this.resetData();
            this.fetchData();
        },
        getTopNews() {
            this.apiUrl = 'https://newsapi.org/v2/top-headlines?country=nz&pageSize=' + this.maxPerPage + '&apiKey=' + this.apiKey;
            this.isBusy = true;
            this.searchword = '';
            this.resetData();
            this.fetchData();
        },
        fetchData(){
            let req = new Request(this.apiUrl + '&page=' + this.currentPage);
            fetch(req)
                .then((resp) => resp.json())
                .then((data) => {
                    this.totalResults = data.totalResults;
                    data.articles.forEach(element => {
                        this.articles.push(element);
                    });
                    this.isBusy = false;
                    this.showLoader = false;
                })
                .catch((error) => {
                    console.error(error);
                });
        },
        scrollTrigger(){
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if(entry.intersectionRatio > 0 && this.currentPage < this.pageCount) {
                        this.showLoader = true;
                        this.currentPage += 1;
                        this.fetchData();
                    }
                })
            });

            observer.observe(this.$refs.infinitescrolltrigger);
        }
    },
    created() {
        this.getTopNews();
    },
    mounted(){
        this.scrollTrigger();
    }
}

</script>

<style>

.container{
    margin: 0 auto;
}

img {
    width: 100%;
    border-bottom: 2px solid black;
    border-radius: 8px 8px 0 0;
  }

.searchbar{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    padding: 10px 0;
    background: blueviolet;
    color: white;
    height: 15vh;
}

.search-content{
    width: 30%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    padding: 10px 0;
       height: 100%;
}


  select{
    margin: 0 auto;
    width: 80%;
    background-color: white;
    border: black 2px solid;
    height: 30%;
    box-shadow: 5px 5px black;
  }

  .confirmButton{
 background-color: white;
    border: black 2px solid;
    box-shadow: 5px 5px black;
  }

  form{
    width: 100%;
    height: 30%;
    
  }

  input{
    width: 100%;
    background-color: white;
    border: black 2px solid;
    height: 80%;
    box-shadow: 5px 5px black;
  }

  p{margin: 0;}



  .icon-container{
    width: 30%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    padding: 5px 0;
  }

.article-results{
    width: 100%;
    text-align: center;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-evenly;
    row-gap: 30px;
    margin: 0 auto;
    padding: 20px 0;
    background-color: antiquewhite;
}

.articleText{
    padding: 10px;
}

article{
    border: 2px solid black;
    box-shadow: 10px 10px black;
    border-radius: 10px;
    width: 25%;
    background-color: white;
}

h3{
    color:blueviolet
}

.artButton{
    color: blueviolet;
    background-color: white;
    border: none;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
}

.artButton:hover{
    color: black;
}

#scroll-trigger{
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    background-color: antiquewhite;
    }

@media screen and (max-width: 800px) {
    
    h3{
        font-size: 15px;
    }
    p{
        font-size: 12px;
    }
    article{
        width: 90%;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items:  center;
    }

    article img{
        width: 40%;
        border-right: 2px solid black;
        border-bottom: none; 
        border-radius: 8px 0 0 8px;   
        }

    

.search-content{
    width: 90%;
    flex-direction: column;

}

    .searchbar{

        flex-direction: column;
        height: 25vh;

    }
}

</style>