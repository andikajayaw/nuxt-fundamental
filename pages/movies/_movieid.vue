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
        </div>
        <div class="movie-content">
          <h1>{{ movie.title }}</h1>
          <p class="movie-fact tagline">
            <span>Tagline: </span>"{{ movie.tagline }}"
          </p>
          <p class="movie-fact">
            <span
              >Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </span>
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span>
            {{
              movie.revenue.toLocaleString('en-us', {
                style: 'currency',
                currency: 'USD',
              })
            }}
          </p>
          <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
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

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 600px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
