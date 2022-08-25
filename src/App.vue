<template>
  <div
    id="app"
    class="bg-dark bg-gradient"
  >
    <Header
      @takeSearch="search($event)"
      @takeGenre="genreSelect($event)"
    />
    <Main
      v-if="loaded"
      :cards="searched"
      :popular="popularMovies"
      :genres="genres"
    />
    <div
      v-else
      class="container-fluid homepage"
    >
      <div class="d-flex h-100">
        <h1 class="text-danger m-auto text-center fs-2">
          Benvenuto in <span class="d-block fs-1">Boolflix</span>
        </h1>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  name: 'App',
  components: {
    Header,
    Main,
  },
  data() {
    return {
      query: 'https://api.themoviedb.org/3/',
      api_key: '89eb092bce881ee73ddbbdbb875f67e8',
      language: 'it',
      searchText: null,
      selectedGenre: '',
      loaded: false,
      searched: {
        films: null,
        series: null,
      },
      popularMovies: {
        films: [],
        series: [],
      },
      genres: {
        movie: null,
        tv: null,
      },
    };
  },
  watch: {
    searchText() {
      this.getFilms();
      this.getSeries();
    },
  },
  mounted() {
    this.getPopularFilms();
    this.getPopularSeries();
    setTimeout(() => {
      this.loaded = true;
    }, 1000);
  },
  methods: {
    search(text) {
      if (text !== '') {
        this.searchText = text;
      }
    },
    genreSelect(text) {
      if (text !== '') {
        this.selectedGenre = text;
      }
    },
    getFilms() {
      const endPoint = 'search/movie';
      const params = {
        api_key: this.api_key,
        language: this.language,
        query: this.searchText,
      };
      axios
        .get(`${this.query}${endPoint}`, { params })
        .then((result) => {
          if (result.data.results !== []) {
            this.searched.films = result.data.results;
            this.searched.films.forEach((element, index) => {
              this.getCast(element.id, index, 'movie', this.searched.films);
            });
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getSeries() {
      const endPoint = 'search/tv';
      const parametersFirst = {
        api_key: this.api_key,
        language: this.language,
        query: this.searchText,
      };
      axios
        .get(`${this.query}${endPoint}`, { params: parametersFirst })
        .then((result) => {
          if (result.data.results !== []) {
            this.searched.series = result.data.results;
            this.searched.series.forEach((element, index) => {
              this.getCast(element.id, index, 'tv', this.searched.series);
            });
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getPopularFilms() {
      const endPoint = 'trending/movie/week';
      const params = {
        api_key: this.api_key,
        language: this.language,
      };
      axios
        .get(`${this.query}${endPoint}`, { params })
        .then((result) => {
          this.popularMovies.films = result.data.results;
          this.popularMovies.films.forEach((element, index) => {
            this.getCast(element.id, index, 'movie', this.popularMovies.films);
          });
          this.getGenres('movie');
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getPopularSeries() {
      const endPoint = 'trending/tv/week';
      const params = {
        api_key: this.api_key,
        language: this.language,
      };
      axios
        .get(`${this.query}${endPoint}`, { params })
        .then((result) => {
          this.popularMovies.series = result.data.results;
          this.popularMovies.series.forEach((element, index) => {
            this.getCast(element.id, index, 'tv', this.popularMovies.series);
          });
          this.getGenres('tv');
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getCast(id, index, type, array) {
      const endPoint = `${type}/${id}/credits`;
      const params = {
        api_key: this.api_key,
        language: this.language,
      };
      axios
        .get(`${this.query}${endPoint}`, { params })
        .then((result) => {
          const cast = [];
          for (let i = 0; i < 5; i += 1) {
            cast.push(result.data.cast[i].name);
          }
          // eslint-disable-next-line no-param-reassign
          array[index].cast = cast;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getGenres(type) {
      const endPoint = `genre/${type}/list`;
      const params = {
        api_key: this.api_key,
        language: this.language,
      };
      axios
        .get(`${this.query}${endPoint}`, { params })
        .then((result) => {
          this.genres[type] = result.data.genres;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap.scss";
#app {
  width: 100%;
  height: 100vh;
  overflow: auto;
  .homepage {
    height: 80vh;
  }
}
</style>
