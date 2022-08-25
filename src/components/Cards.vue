<template>
  <div
    class="row gap-3 justify-content-center my-3 text-center p-3"
    :class="(!activeFilms) ? 'show_off' : ''"
  >
    <div
      v-if="list.length > 0"
      class="col-12"
    >
      <div class="container-fluid d-flex justify-content-between align-items-end my_nav_menu">
        <h1>{{ title }}</h1>
        <h5
          v-show="!activeFilms"
          @click="activeFilms = !activeFilms"
        >
          Sfoglia tutti
        </h5>
        <h5
          v-show="activeFilms"
          @click="activeFilms = !activeFilms"
        >
          Nascondi
        </h5>
      </div>
    </div>
    <div
      v-else
      class="col-12"
    >
      <h2>
        {{ title }}: Nessun risultato
      </h2>
    </div>
    <Card
      v-for="(film) in list"
      :key="film.id"
      :list="film"
      :type="type"
      :genres="genres"
      :selected-genre="selectedGenre"
    />
  </div>
</template>

<script>

import Card from './Card.vue';

export default {
  name: 'Cards',
  components: {
    Card,
  },
  props: {
    list: {
      type: Array,
      default() {
        return null;
      },
    },
    type: {
      type: String,
      default: '',
    },
    title: {
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
      activeFilms: false,
      activeSeries: false,
    };
  },
};
</script>

<style lang="scss" scoped>
.show_off {
  height: 540px;
  overflow: hidden;
}

.my_nav_menu {
  h5:hover {
    cursor: pointer;
    text-decoration: underline;
  }
}
</style>
