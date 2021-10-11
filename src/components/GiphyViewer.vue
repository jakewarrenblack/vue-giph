<template>
    <div>

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
    </div>
</template>

<script>
    import axios from 'axios';
    const GIPHY_URL = "https://api.giphy.com/v1/gifs";
    const API_KEY = "j93AkA81GMU3oT6p64nUz6jTOyhLNJi7";

    export default{
        name: 'GiphyViewer',
        data() {
            // data needs to return an object
            return {
                gifs: []
            };
        },
        // mounted will occur before methods, convention is to put it before them in the code
        mounted(){
            //  this is called a 'chained' method
            axios.get(`${GIPHY_URL}/trending?api_key=${API_KEY}`)
                // nice to put 'response' within soft brackets, in case you might be adding a second parameter in the future
                // wrapping the return of an arrow functon in curly braces means you can do things before returning anything
                // eg {doThis; doThat; return something;}
                .then((response) => {
                    console.log(response.data.data);
                    this.gifs = response.data.data;
                })
                .catch(error => console.log(error))
        },
        methods: {
            // search method will go in here
        }
    }
</script>

<style>

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