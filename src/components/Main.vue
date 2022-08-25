<template>
  <main class="py-1">
    <div class="container-fluid">
      <div class="row">
        <div class="col-2">
          <select
            v-show="getAllGenres()"
            id="genresSelect"
            v-model="selectedGenre"
            class="form-select m-4"
            aria-label="Disabled select example"
            name="genres"
          >
            <option
              selected
              value=""
            >
              Filtra per genere
            </option>
            <option
              v-for="genre in allGenres"
              :key="genre + 1"
              :value="genre"
            >
              {{ genre }}
            </option>
          </select>
        </div>
      </div>
    </div>
    <div
      v-if="cards.films || cards.series"
      class="container-fluid text-white"
    >
      <Cards
        v-show="cards.films"
        :list="cards.films"
        :title="'Film'"
        :type="'movie'"
        :genres="genres"
        :selected-genre="selectedGenre"
      />
      <Cards
        v-show="cards.series"
        :list="cards.series"
        :title="'Serie TV'"
        :type="'tv'"
        :genres="genres"
        :selected-genre="selectedGenre"
      />
    </div>
    <div
      v-else
      class="container-fluid text-white"
    >
      <Cards
        v-show="popular.films"
        :list="popular.films"
        :title="'Film Popolari'"
        :type="'movie'"
        :genres="genres"
        :selected-genre="selectedGenre"
      />
      <Cards
        v-show="popular.series"
        :list="popular.series"
        :title="'Serie TV Popolari'"
        :type="'tv'"
        :genres="genres"
        :selected-genre="selectedGenre"
      />
    </div>
  </main>
</template>

<script>
import Cards from './Cards.vue';

export default {
  name: 'Main',
  components: {
    Cards,
  },
  props: {
    cards: {
      type: Object,
      default() {
        return {};
      },
    },
    popular: {
      type: Object,
      default() {
        return {};
      },
    },
    genres: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      selectedGenre: '',
      allGenres: [],
    };
  },
  methods: {
    getAllGenres() {
      this.genres.movie.forEach((element) => {
        if (!this.allGenres.includes(element.name)) {
          this.allGenres.push(element.name);
          console.log(element.name);
        }
      });
      this.genres.tv.forEach((element) => {
        if (!this.allGenres.includes(element.name)) {
          this.allGenres.push(element.name);
          console.log(element.name);
        }
      });
      return true;
    },
  },
};
</script>

<style scoped lang="scss">

</style>
