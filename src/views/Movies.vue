<template>
  <div class="about container">
    <Search />
    <button @click="page+=1, load()" class="btn btn-success mx-2 m-4" >Next Movies</button>
    <button type="button" class="btn btn-primary">
      Current Page <span class="badge bg-secondary">{{page}}</span>
    </button>
    <button v-for="(eachpage,index) in allpages"  :key="index" @click="page=eachpage, load()">{{eachpage}}</button>
    <button @click="page-=1, load()" class="btn btn-warning mx-2 m-4" >Prev Previous Movies</button>
    <div class="row">
        <div v-for="movie in movies" :key="movie.id" class="col-lg-3 col-xm-4">
          <img :src="'https://image.tmdb.org/t/p/original/' + movie.poster_path" alt="" width="250">
          <p>{{movie.original_title}}</p>
        </div>
    </div>
    
  </div>
</template>

<script>
  import axios from "axios"
  import {ref,onMounted,watch} from "vue"
  import Search from "@/components/Search.vue"
  export default{
    components:{
      Search
    },
    setup() {
      //Datas
      let movies = ref([])
      let search = ref('')
      let page = ref(1)
      let allpages = ref(10)
    
      
      // Methods
       function load(){
         axios.get(`https://api.themoviedb.org/3/discover/movie?api_key=fe41c35f11807dde440f1e415d101ec9&sort_by=popularity.desc&with_genres=28&page= ${page.value}`)
         .then((res)=>{
           console.log(res.data.results)
           movies.value =  res.data.results
         
         })
         .catch((error)=>{
           console.log(error)
         })
         
       }

        onMounted(() => {
        load()
      })

      watch(page, (newPage,oldPage)=>{
        if(oldPage && newPage < 1){
          page.value = 1
          alert('You are on the Last Page')
        }
      })
        return{
          movies,
          search,
          page,
          load,
          allpages
          
         
        }

    },

   
  }
</script>
