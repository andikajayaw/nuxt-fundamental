<template>
  <div class="home">
    <!-- Hero -->
    <Hero />

    <!-- Search -->
    <div class="container search">
      <input
        v-model.lazy="search"
        type="text"
        placeholder="Search..."
        @keyup.enter="$fetch"
      />
      <button v-show="search.length > 0" class="button" @click="clearSearch">
        X
      </button>
    </div>

    <Loading v-if="$fetchState.pending" />

    <!-- Movies -->
    <MovieCard v-else :list-movie="movies" />
  </div>
</template>

<script>
import axios from 'axios'
export default {
  // loading: false,
  data() {
    return {
      movies: [],
      search: '',
    }
  },
  async fetch() {
    await this.getMovies()
  },
  fetchDelay: 1000,
  methods: {
    async getSearchMovies() {
      const res = await axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=637229d93973db648ccec07e3acd8553&language=en-US&page=1&query=${this.search}`
        )
        .then((response) => response.data)
        .catch((error) => alert(error.message))
      this.movies = []
      res.results.forEach((element) => {
        this.movies.push(element)
      })
    },
    async getMovies() {
      if (this.search.length > 0) {
        await this.getSearchMovies()
        return
      }
      const data = await axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=637229d93973db648ccec07e3acd8553&language=en-US&page=1'
      )
      const { results } = data.data
      this.movies = []
      results.forEach((element) => {
        this.movies.push(element)
      })
    },
    async clearSearch() {
      this.search = ''
      await this.$fetch()
    },
  },
}
</script>

<style lang="scss" scoped>
.loading {
  padding-top: 120px;
  align-items: flex-start;
}
.search {
  display: flex;
  padding: 32px 16px;
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
  }
}
</style>
