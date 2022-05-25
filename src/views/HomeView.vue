<template>
  <form @submit.prevent="getMovies">
    <input type="text" placeholder="search movie..." v-model="search" />
    <button><img src="../assets/search-icon.svg" alt="" /></button>
  </form>
  <div class="movies">
    <div class="card" v-for="movie in movies" :key="movie.imdbID">
      <router-link :to="'/movie/' + movie.imdbID">
        <div class="img-container">
          <img :src="movie.Poster" alt="" />
        </div>
        <div class="info">
          <h3>{{ movie.Title }}</h3>
          <p>{{ movie.Year }}</p>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";

export default {
  name: "HomeView",
  setup() {
    const search = ref("");
    const movies = ref([]);

    const getMovies = async () => {
      const res = await fetch(
        `http://www.omdbapi.com/?apikey=de75309e&s=${search.value}`
      );
      const data = await res.json();
      movies.value = data.Search;
    };
    return { search, movies, getMovies };
  },
};
</script>

<style lang="scss" scoped>
@import "@/styles/main.scss";

form {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin: 20px;

  input {
    width: 210px;
    height: 50px;
    padding: 15px;
    font-size: 18px;
    border: 1px solid transparent;
    background: var(--bg-inp);
    border-radius: 8px;
    outline: none;
    margin: auto 5px;

    @media (min-width: 400px) {
      width: 350px;
    }
  }

  button {
    width: 50px;
    height: 50px;
    background: var(--bg-btn);
    border: 1px solid transparent;
    border-radius: 8px;
    outline: none;
    cursor: pointer;

    @media (min-width: 400px) {
      width: 60px;
    }
  }
}

.movies {
  display: grid;
  justify-items: start;
  grid-template-columns: repeat(1, 1fr);
  width: max(90%, 300px);
  align-items: center;
  margin: 0 auto 30px;
  align-content: center;

  @media (min-width: 950px) {
    grid-template-columns: repeat(3, 1fr);
  }
}

.card {
  margin: 0 auto 50px;
  padding: 0 5px;
  height: 500px;

  .img-container {
    border: 1px solid transparent;
    border-radius: 8px;
    width: 300px;
    height: 420px;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  .info {
    margin-top: 5px;
    h3 {
      font-size: 18px;
      font-weight: 700;
      color: var(--cl-text);
      max-width: 200px;
    }

    p {
      font-size: 16px;
      font-weight: 700;
      color: var(--cl-year);
    }
  }
}
</style>
