<template>
  <header>
    <div class="container-fluid p-0">
      <nav class="navbar navbar-dark bg-dark">
        <div class="container-fluid">
          <div class="my_nav d-flex align-items-end">
            <img
              src="../assets/logo.png"
              alt="logo"
              class="mx-2 logo"
              @click="reloadPage"
            >
            <ul class="list-inline text-white fs-4 mx-2">
              <li class="list-inline-item me-4 active">
                <a href="#">Home</a>
              </li>
              <li class="list-inline-item me-4">
                <a href="#">Serie Tv</a>
              </li>
              <li class="list-inline-item me-4">
                <a href="#">Film</a>
              </li>
              <li class="list-inline-item me-4">
                <a href="#">Nuovi e popolari</a>
              </li>
              <li class="list-inline-item me-4">
                <a href="#">La mia lista</a>
              </li>
            </ul>
          </div>
          <div class="d-flex">
            <input
              v-model="searched"
              class="form-control me-2 "
              type="search"
              placeholder="Cerca un film"
              aria-label="Search"
              @keyup.enter="$emit('takeSearch', searched)"
            >
            <button
              class="btn btn-outline-success"
              type="submit"
              @click="$emit('takeSearch', searched)"
            >
              <i class="fas fa-search" />
            </button>
          </div>
        </div>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      searched: '',
    };
  },
  methods: {
    reloadPage() {
      window.location.reload();
    },
  },
};
</script>

<style scoped lang="scss">
.my_nav {
  .logo{
    cursor: pointer;
  }
  li {
    a {
      text-decoration: none;
      color: inherit;
      &:hover{
        text-decoration: underline;
      }
    }
    &.active a {
      font-weight: bold;
    }
  }
}
</style>
