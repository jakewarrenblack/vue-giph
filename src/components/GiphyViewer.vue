<template>
    <b-container>
        <b-row no-gutters class="search-box" align-h="between">
            <input type="text" v-model="term" v-on:keyup.enter="searchGiphy()"/>
            <div>
                <b-button mr-3 class="" variant="primary" @click="searchGiphy('search')">Search</b-button>
                <b-button class="" variant="primary" @click="searchGiphy('trending')">Trending</b-button>
                <b-button class="" variant="primary" @click="searchGiphy('random')">Random</b-button>
            </div>
            <div>
                <b-dropdown v-model="limit" id="dropdown-1" text="Limit" class="m-md-2">
                    <li v-for="option in limit" :key="option">
                        <b-dropdown-item @click="selectedLimit = option">{{ option }}</b-dropdown-item>
                    </li>
                </b-dropdown>
            </div>
        </b-row>
        <b-card-group columns>
            <b-card
                v-for="gif in gifs"
                :key="gif.id"
                :img-src="gif.images.fixed_width.url"
                :img-alt="gif.title"
            >
                <b-card-text>
                    <a :href="gif.url" target="_blank">{{ gif.title }}</a>
                </b-card-text>
            </b-card>
        </b-card-group>
    </b-container>
</template>

<script>
    import axios from 'axios';
    const GIPHY_URL = "https://api.giphy.com/v1/gifs";
    const API_KEY = "j93AkA81GMU3oT6p64nUz6jTOyhLNJi7";

    export default {
        name: 'GiphyViewer',
        data() {
            // data needs to return an object
            return {
                gifs: [],
                // our search term
                term: "",
                limit: [1,20,50],
                selectedLimit: 50,
            }
        },
        // mounted will occur before methods, convention is to put it before them in the code
        mounted(){
            this.trendingSearch();
        },
        methods: {   
            // TODO: Combine into a generic search?
            trendingSearch(){
            //  this is called a 'chained' method
                axios.get(`${GIPHY_URL}/trending?api_key=${API_KEY}`)
                // nice to put 'response' within soft brackets, in case you might be adding a second parameter in the future
                // wrapping the return of an arrow functon in curly braces means you can do things before returning anything
                // eg {doThis; doThat; return something;}
                .then((response) => {
                    this.gifs = response.data.data;
                })
                .catch(error => console.log(error))
            },  
            randomSearch(){
                this.gifs = [];
                axios.get(`${GIPHY_URL}/random?api_key=${API_KEY}`)
                .then((response) => {                 
                    this.gifs = [response.data.data];
                })
                .catch(error => console.log(error))
            },
            searchGiphy(type){
                console.log(type);
                switch(type){
                    case('search'):

                    if(!this.term){
                        // alert("Please enter a search term.");
                        this.$bvToast.toast('Please enter a search term.', {
                            title: 'Warning',
                            variant: 'danger',
                            toaster: 'b-toaster-top-center',
                            'autoHideDelay':5000,
                            solid: true                    
                        })
                        return;
                    }
                    axios.get(`${GIPHY_URL}/search?api_key=${API_KEY}&q=${this.term}&limit=${this.selectedLimit}`)
                    .then((response) => {
                        this.gifs = response.data.data;
                    })            
                    this.term = "";
                    break;

                    case('trending'):
                    this.trendingSearch();
                    this.term = "";
                    break;

                    case('random'):
                    this.randomSearch();
                    break;

                }
                
                
            },
        }
    }
</script>

<style>

    .search-box{
        margin: 20px 0;        
    }


    .card{
        margin-bottom: 20px;
    }

    @media (min-width: 34em) {
        .card-columns {
            -webkit-column-count: 2;
            -moz-column-count: 2;
            column-count: 2;
        }
    }
    @media (min-width: 48em) {
        .card-columns {
            -webkit-column-count: 3;
            -moz-column-count: 3;
            column-count: 3;
        }
    }
    @media (min-width: 62em) {
        .card-columns {
            -webkit-column-count: 4;
            -moz-column-count: 4;
            column-count: 4;
        }
    }
    @media (min-width: 75em) {
        .card-columns {
            -webkit-column-count: 5;
            -moz-column-count: 5;
            column-count: 5;
        }
    }
</style>