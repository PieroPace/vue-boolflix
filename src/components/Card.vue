<template>
  <div
    v-show="genreList.includes(selectedGenre) || selectedGenre == ''"
    class="card col-2 p-0"
    @mouseover="active = true"
    @mouseleave="active = false"
  >
    <div
      class="card-body"
      :class="(active) ? 'd-none': ''"
    >
      <img
        v-if="list.poster_path"
        :src="`https://image.tmdb.org/t/p/original${list.poster_path}`"
        class="card-img-top"
        :alt="title"
      >
      <img
        v-else
        src="../assets/stock_image.png"
        class="card-img-top"
        :alt="title"
      >
    </div>
    <div
      :class="(!active) ? 'd-none': ''"
      class="card-body card-description"
    >
      <ul class="list-group list-group-flush">
        <li class="list-group-item border-0">
          <h1 class="fs-5">
            Titolo
          </h1>
          <p>{{ title }}</p>
        </li>
        <li
          v-show="title !== originalTitle"
          class="list-group-item border-0"
        >
          <h1 class="fs-5">
            Titolo Originale
          </h1>
          <p>{{ originalTitle }}</p>
        </li>
        <li
          v-show="list.overview"
          class="list-group-item border-0"
        >
          <h1 class="fs-5">
            Trama
          </h1>
          <p>{{ list.overview }}</p>
        </li>
        <li
          v-show="list.cast"
          class="list-group-item border-0"
        >
          <h1 class="fs-5">
            Attori principali
          </h1>
          <p
            v-for="actor in list.cast"
            :key="list.id+actor"
          >
            {{ actor }}
          </p>
        </li>
        <li
          v-show="list.genre_ids"
          class="list-group-item border-0"
        >
          <h1 class="fs-5">
            Generi
          </h1>
          <p
            v-for="genre in list.genre_ids"
            :key="list.id+genre"
            class="text-left"
          >
            {{ getGenre(genre) }}
          </p>
        </li>
        <li class="list-group-item border-0">
          <h1 class="fs-5">
            Lingua
          </h1>
          <i :class="'flag flag-' + getFlag(list.original_language)" />
        </li>
        <li class="list-group-item border-0">
          <h1 class="fs-5">
            Voto
          </h1>
          <i
            v-for="n in 5"
            :key="n"
            :class="(n <= roundNumber(list.vote_average)) ? 'fas fa-star' : 'far fa-star'"
          />
        </li>
        <li class="list-group-item border-0">
          <h1 class="fs-5">
            <a
              :href="`https://www.themoviedb.org/${getUrl()}`"
              target="_blank"
            >Cerca su TMDB</a>
          </h1>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    list: {
      type: Object,
      default() {
        return {};
      },
    },
    type: {
      type: String,
      default: '',
    },
    genres: {
      type: Object,
      default() {
        return {};
      },
    },
    selectedGenre: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      active: false,
      genreList: [],
      title: (this.type === 'movie') ? this.list.title : this.list.name,
      originalTitle: (this.type === 'movie') ? this.list.original_title : this.list.original_name,
    };
  },
  methods: {
    getFlag(lang) {
      switch (lang) {
        case 'en':
          return 'us';
        case 'ko':
          return 'kr';
        case 'ja':
          return 'jp';
        case 'ur':
          return 'pk';
        case 'zh':
          return 'cn';
        default:
          return lang;
      }
    },
    roundNumber(num) {
      return Math.round(num / 2);
    },
    getUrl() {
      return `${this.type}/${this.list.id}`;
    },
    getGenre(genreId) {
      const genresObj = this.genres;
      let myGenre = '';
      for (let i = 0; i < genresObj.movie.length; i += 1) {
        const movieElement = genresObj.movie[i];
        if (movieElement.id === genreId) {
          myGenre = movieElement.name;
          this.genreList.push(myGenre);
        }
      }
      for (let i = 0; i < genresObj.tv.length; i += 1) {
        const movieElement = genresObj.tv[i];
        if (movieElement.id === genreId) {
          myGenre = movieElement.name;
          this.genreList.push(myGenre);
        }
      }
      return myGenre;
    },
  },
};
</script>

<style scoped lang="scss">
@import '~mdb-ui-kit/css/mdb.min.css';
.card {
  cursor: pointer;
  .card-description {
    height: 440px;
    overflow: auto;
  }
}
</style>
