<template>
<div class="home">
  <!-- hero -->
  <Hero/>

<!-- search -->
<div class="container search">
  <input  v-model.lazy="searchInput"  type="text" placeholder="press enter after type" @keyup.enter="$fetch"/>
  <button v-show="searchInput !== ''" class="button" @click="clearSearch">clear search</button>
</div>

<!-- loading -->
<Loading v-if="$fetchState.pending"/>

  <!-- movies -->
  <div v-else class="container movies"> 
    <!-- searched movies -->
    <div v-if="searchInput !== ''" id="movies-grid" class="movies-grid">
      <div v-for="(movie,index) in searchedMovies" :key="index" class="movie">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
          <p class="review">{{movie.vote_average}}</p>
          <p class="overview">{{movie.overview}}</p>
        </div>
        <div class="info">
          <p class="title">
            {{movie.title.slice(0,25)}}
            <span v-if="movie.title.length > 25">...</span>
          </p>
          <p class="release">
            Released: 
            {{
              new Date(movie.release_date).toLocaleString('en-US',{
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>
          <NuxtLink class="button button-light" :to="{name: 'movies-movieid', params:{movieid:movie.id}}">Get More Info</NuxtLink>
        </div>
      </div>
    </div>

    <!-- now streaming -->
    <div v-else id="movies-grid" class="movies-grid">
      <div v-for="(movie,index) in movies" :key="index" class="movie">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
          <p class="review">{{movie.vote_average}}</p>
          <p class="overview">{{movie.overview}}</p>
        </div>
        <div class="info">
          <p class="title">
            {{movie.title.slice(0,25)}}
            <span v-if="movie.title.length > 25">...</span>
          </p>
          <p class="release">
            Released: 
            {{
              new Date(movie.release_date).toLocaleString('en-US',{
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>
          <NuxtLink class="button button-light" :to="{name: 'movies-movieid', params:{movieid:movie.id}}">Get More Info</NuxtLink>
        </div>
      </div>
    </div>
  </div>
  <Footer/>
</div>
</template>

<script>
import axios from 'axios';

export default {
  data(){
    return{
      movies:[],
      searchedMovies:[],
      searchInput: ''
    }
  },
  async fetch(){
    if(this.searchInput === ''){
    await this.getMovies()
    return
    }
      
    await this.getSearchedMovies()
    
  },
  head(){
    return{
      title : 'Movie App - Streaming New Movie Info',
      meta: [
        {
          hid:'description',
          name:'description',
          content:'Get all info about movie streaming'
        },
        {
          hid:'keywords',
          name:'keywords',
          content:'movies,stream,streaming,info movie'
        }
      ]
    }
  },
  fetchDelay : 1000,
  methods: {
  async getMovies(){
    const data = axios.get("https://api.themoviedb.org/3/movie/now_playing?api_key=d7a43991c351fc435f06627114f15aba&language=en-US&page=1")
    const result = await data
    result.data.results.forEach(movie=>{
      this.movies.push(movie)
    })
  },
  async getSearchedMovies(){
    const data = axios.get(`https://api.themoviedb.org/3/search/movie?api_key=d7a43991c351fc435f06627114f15aba&language=en-US&page=1&query=${this.searchInput}`)
    const result = await data
    result.data.results.forEach(movie => {
    this.searchedMovies.push(movie)
  })
  },
  clearSearch() {
    this.searchInput = ''
    this.searchedMovies = []
  }
}
}
</script>

<style lang="scss">
.home {
  background-color: rgb(33,31,31);
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    padding: 32px 4rem;
    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;
      &:focus {
        outline: none;
      }
    }
    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
      border-color:#11ae91;
      background-color: #11ae91;
    }
  }
  .movies {
    padding: 32px 4rem;

    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;

      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .movie-img {
          position: relative;
          overflow: hidden;
          &:hover {
            .overview {
              transform: translateY(0);
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: #11ae91;
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }
          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: #11ae91;
            padding: 12px;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
          }
        }
        .info {
          margin-top: auto;
          .title {
            margin-top: 8px;
            color: #fff;
            font-size: 20px;
          }
          .release {
            margin-top: 8px;
            color: #c9c9c9;
          }
          .button {
            margin-top: 8px;
            border-color:#11ae91;
          }
        }
      }
    }
  }
}
</style>