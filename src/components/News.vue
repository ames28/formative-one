<template>
    <div class="container">
        <div class="searchbar">
            <form @submit.prevent="fetchSearchNews">
                <input type="text" placeholder="search..." v-model="searchword">
            </form>
            <div class="search-icons">
                <i v-if="!isBusy" class="fas fa-search" @click="fetchSearchNews"></i>
                <i v-else class="fas fa-spinner fa-spin"></i>
                <i class="fas fa-times" @click="fetchTopNews"></i>
            </div>
        </div>
        <div class="result-list">
           <article v-for="(article, index) in articles" :key="index" @click="navTo(article.url)">
                <header>
                    <img v-if="article.urlToImage" :src="article.urlToImage" alt="">
                    <i v-else class="fas fa-image"></i>
                    <h2>{{article.title}}</h2>
                    <h5>{{article.author}} || {{article.source.name}}</h5>
                    <p>{{article.description}}</p>
                </header>
                
                <footer>
                <i class="fas fa-chevron-right"></i>
                </footer>
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
            maxPerPage: 20,
            searchword: '',
            articles: [],
            country: 'nz',
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
        fetchSearchNews() {
            if(this.searchword !== '') {
                this.apiUrl = 'https://newsapi.org/v2/everything?q=' + this.searchword + '&pageSize=' + this.maxPerPage + '&apiKey=' + this.apiKey;
                this.isBusy = true;

                this.resetData();
                this.fetchData();
            }
            else{
                this.fetchTopNews();
            }
            
        },
        fetchTopNews() {
            this.apiUrl = 'https://newsapi.org/v2/top-headlines?country=' + this.country + '&pageSize=' + this.maxPerPage + '&apiKey=' + this.apiKey;
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
                // .catch((error) => {
                //     console.log('Error');
                // })
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
        this.fetchTopNews();
    },
    mounted(){
        this.scrollTrigger();
    }
}

</script>

<style>

</style>