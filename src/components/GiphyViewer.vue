<template>
    <b-container>
        <b-row no-gutters class="search-box" align-h="between">
            <input type="text" v-model="term" v-on:keyup.enter="searchGiphy()"/>
            <div>
                <b-button mr-3 class="" variant="primary" @click="genericSearch('search')">Search</b-button>
                <b-button class="" variant="primary" @click="genericSearch('trending')">Trending</b-button>
                <b-button class="" variant="primary" @click="genericSearch('random')">Random</b-button>
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
            this.genericSearch('trending');
        },
        methods: {
            genericSearch(type){
                if(type == 'search'){
                        if(!this.term){
                        this.$bvToast.toast('Please enter a search term.', {
                            title: 'Warning',
                            variant: 'danger',
                            toaster: 'b-toaster-top-center',
                            'autoHideDelay':5000,
                            solid: true                    
                        })
                        return;
                    }
                }
                // 'something || 'do this' is the opposite of the ?? operator                 
                axios.get(`${GIPHY_URL}/${type}?api_key=${API_KEY}${type == 'search' ? '&q=' + this.term : ''}${'&limit=' + this.selectedLimit}`)
                .then((response) => {
                    console.log(response)
                    type == 'random' ? this.gifs = [response.data.data] : this.gifs = response.data.data;
                })
                .catch(error => console.log(error))
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