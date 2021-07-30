<template lang="html">
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt3896198">
        <img
          src="https://m.media-amazon.com/images/M/MV5BNjM0NTc0NzItM2FlYS00YzEwLWE0YmUtNTA2ZWIzODc2OTgxXkEyXkFqcGdeQXVyNTgwNzIyNzg@._V1_SX300.jpg"
          alt="Guardians of the Galaxy Vol. 2 Poster"
          class="featured-img"
        />
        <div class="details">
          <h3>Guardians of the Galaxy Vol. 2</h3>
          <p>
            The Guardians struggle to keep together as a team while dealing with
            their personal family issues, notably Star-Lord's encounter with his
            father the ambitious celestial being Ego.
          </p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for?"  v-model = 'search' />
      <input type="submit" value="search" />
    </form>

    <div class="movie-list">
      <div class="movie" v-for='movie in movies' :key='movie.imdbID'>
        <router-link :to="'/movie/' + movie.imdbID" class='movie-link'>
          <div class="product-image">
            <img :src="movie.Poster" alt="movie poster">
            <div class="type">{{movie.Type}}</div>
          </div>
          <div class="detail">
            <p class='Y'>{{movie.Year}}</p>
            <h3>{{movie.Title}}</h3>
          </div>

        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env.js" ;


export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

     const SearchMovies = () => {
      if (search.value != "") {
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then(response => response.json())
          .then(data => {
            movies.value = data.Search ;
            search.value = '';
            console.log(movies.value)
          });
      }
    };




    return {
      search,
      movies,
      SearchMovies
    };
  },
};
</script>

<style lang="scss">
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .details {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      z-index: 1;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        background-color: #496583;

        &::placeholder {
          color: #fff;
        }
      }

      &[type="submit"] {
        width: 100%;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        background-color: #42b883;
        max-width: 300px;
        text-transform: uppercase;

        &:active {
          background-color: #3bb070;
        }
      }
    }
  }
  .movie-list {
    display:flex;
    flex-wrap:wrap;
    margin: 0px 8px;

    .movie{
      max-width:50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link{
        display:flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display:block;
            width:100%;
            height: 275px;
            object-fit: cover;
          }

          .type{
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #fff;
            bottom:16px;
            left: 0px ;
            text-transform: capitalize;
          }
        }

        .detail{
          background-color: #496583;
          padding: 16px 8px;
          flex:1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .Y {
            color:#AAA;
            font-weight: 600;
            font-size: 10px
          }

          h3{
            font-size:18px;
            color:#fff;
            font-weight: 600;
          }
        }


      }
    }
  }
}
</style>
