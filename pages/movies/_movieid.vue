<template>
  <div>
    <Loading v-if="$fetchState.pending" />
    <div v-else class="container single-movie">
      <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>
      <div class="movie-info">
        <div class="movie-img">
          <img
            v-if="movie.poster_path != null"
            :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
            alt="movie_poster"
          />
          <img v-else src="../../assets/imgs/no-img.jpg" alt="no-poster" />
          <p class="review">{{ movie.vote_average }}</p>
          <p class="overview">{{ movie.overview }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'SingleMovie',
  data() {
    return {
      movie: null,
    }
  },
  async fetch() {
    await this.getMovieDetail()
  },
  fetchDelay: 1000,
  methods: {
    async getMovieDetail() {
      const res = await axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=637229d93973db648ccec07e3acd8553&language=en-US`
      )
      console.log(res.data)
      this.movie = res.data
    },
  },
}
</script>

<style lang="scss" scoped></style>
