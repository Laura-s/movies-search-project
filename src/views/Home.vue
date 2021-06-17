<template>
  <div class="container">
    <div class="row justify-content-center py-5" id="search-row">
      <div class="col-5">
        <div class="input-group">
          <input
            type="text"
            id="input-title"
            class="form-control"
            placeholder="Title"
            aria-label="Title"
            aria-describedby="button-addon2"
            @keydown.enter="getResults"
            v-model="searchTerm"
          />
          <button
            class="btn btn-outline-primary"
            type="button"
            id="button-addon2"
            v-bind:class="{
              loading: isLoading,
              disabled: searchTerm.trim() === '',
            }"
            @click="getResults"
          >
            <span
              class="spinner-border spinner-border-sm"
              role="status"
              aria-hidden="true"
            ></span>
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="row" id="results-row">
      <MovieCard v-for="movie in movies" :key="movie.id" :movie="movie" />
    </div>
  </div>
  <Loader v-show="isLoading" />
</template>

<script>
import MovieCard from "../components/movie-card";
import Loader from "../components/loader.vue";

export default {
  data: () => ({
    isLoading: false,
    searchTerm: "",
    movies: [],
    cat: false,
  }),
  methods: {
    getResults() {
      if (this.searchTerm.trim() !== "") {
        const url =
          "https://imdb8.p.rapidapi.com/title/find?q=" + this.searchTerm.trim();
        const options = {
          method: "GET",
          headers: {
            "x-rapidapi-key":
              "4e037fb5a4mshd017cdd280e6974p11ccbdjsna16daae232bc",
            "x-rapidapi-host": "imdb8.p.rapidapi.com",
          },
        };
        this.isLoading = true;

        fetch(url, options)
          .then((response) => response.json())
          .then((data) => {
            this.isLoading = false;
            this.movies = data.results.filter(
              (movie) => movie.titleType == "movie"
            );
          });
      }
    },
    
  },

  components: {
    MovieCard,
    Loader,
  },
};
</script>

<style>
#search-row .spinner-border {
  display: none;
}

#search-row .loading .spinner-border {
  display: inline-block;
}

.card-body .card-title {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card img {
  height: 300px;
  object-fit: cover;
}
#button-addon2 {
  width: 100px;
}

.capitalize {
  text-transform: capitalize;
}
</style>
