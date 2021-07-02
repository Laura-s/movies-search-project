<template>

  <div v-if="movie" class="container mt-4">
    <div class="row">
      <div class="col-sm-12 col-md-6 bg-light p-md-2">
        <p @click="removeFavourite" class="text-end heart" v-if="isFavourite">
          🧡
        </p>
        <p @click="addFavourite" v-else class="text-end heart">🤍</p>
        <img class="img-fluid" :src="movie.title.image.url" alt="..." />
      </div>
      <div class="col-sm-12 col-md-6   p-3 p-md-5">
        <div class="d-flex align-items-center mb-3">
          <h3 class="me-1 fw-bold m-0">
            {{ movie.title.title }}
          </h3>
          <p class="m-0">({{ movie.title.year || "n/a" }})</p>

          <div class="ms-auto text-muted">
            <p class="raiting">Rating: {{ movie.ratings.rating }}/10</p>
            <p class="votes">{{ movie.ratings.ratingCount }} votes</p>
          </div>
        </div>
        <div class="text-start">
          <p>Summary:</p>

          <p>{{ movie.plotSummary.text }}</p>
        </div>
      </div>
    </div>
  </div>
  <Loader v-show="isLoading" />
</template>

<script>
import Loader from "../components/loader.vue";
export default {
  data: () => ({
    movie: null,
    isFavourite: false,
    isLoading: true,
  }),
  created() {
    this.getMovieDetailes();
    // this.checkFavs()
  },
  methods: {
    getMovieDetailes() {
      this.isLoading = true
      fetch(
        "https://imdb8.p.rapidapi.com/title/get-overview-details?tconst=" +
          this.$route.params.id,
        {
          method: "GET",
          headers: {
            "x-rapidapi-key":
              "4e037fb5a4mshd017cdd280e6974p11ccbdjsna16daae232bc",
            "x-rapidapi-host": "imdb8.p.rapidapi.com",
          },
        }
      )
        .then((response) => response.json())
        .then((response) => {
          this.isLoading = false
          this.movie = response;
          this.checkFavs();
        })
        .catch((err) => {
          this.isLoading = false
          console.error(err);
        });
    },
    addFavourite() {
      const favs = JSON.parse(localStorage.getItem("favourites"));
      favs.push(this.movie.id);
      localStorage.setItem("favourites", JSON.stringify(favs));
      this.checkFavs();
    },
    removeFavourite() {
      const favs = JSON.parse(localStorage.getItem("favourites"));
      const indexOfitem = favs.indexOf(this.movie.id);
      favs.splice(indexOfitem, 1);
      localStorage.setItem("favourites", JSON.stringify(favs));
      this.checkFavs();
    },
    checkFavs() {
      const favs = JSON.parse(localStorage.getItem("favourites"));
      this.isFavourite = favs && favs.includes(this.movie.id);
    },
  },
  components:{
     Loader,
  }
};
</script>

<style scoped>
img {
  max-height: 500px;
}
.raiting {
  font-size: 14px;
  margin: 0;
}
.votes {
  font-size: 12px;
  margin: 0;
}
.heart {
  margin: 0;
  cursor: pointer;
}
</style>