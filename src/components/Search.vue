<template lang="">
    <div>
        Search
        <input type="text" @keyup="getMovies(query)" v-model="query">
        <div class="row">
            <div v-for="movie in results" :key="movie.id" class="col-lg-3 col-xm-4">
                <img :src="'http://image.tmdb.org/t/p/w500/' + movie.poster_path" alt="" width="250">
                <p>{{movie.original_title}}</p>
            </div>
        </div>
    </div>
</template>
<script>
    import axios from "axios"
    import {ref} from "vue"
export default {
    name:"search",

    setup() {
        let query =  ref('');
        let results = ref('')

        function getMovies(query){
            axios.get('https://api.themoviedb.org/3/search/movie?api_key=fe41c35f11807dde440f1e415d101ec9&query=' + query)
            .then((res)=>{
                console.log(res.data)
                results.value = res.data.results
                
            })
            .catch((error)=>{
                console.log(error)
            })
        }

        return{
            query,
            results,
            getMovies
        }
    }
}
</script>
<style lang="">
    
</style>