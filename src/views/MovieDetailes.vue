<template>
  <div v-if="movie">
    <img :src="movie.title.image.url" alt="..." />
    <h5>
      {{ movie.title.title }}
    </h5>
    <p>{{ movie.title.year || "n/a" }}</p>
    <!-- <div v-if="rating"> -->
      <p>
        Rating: {{ movie.ratings.rating }}/10 <br />
        {{ movie.ratings.ratingCount }} votes
      </p>
    <!-- </div> -->

    <!-- <p v-for="synops in synopses" :key="synops.id">{{ synops.text }}</p> -->
    <p>{{movie.plotSummary.text}}</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    movie: null,
  }),
  created() {
    this.getMovieDetailes();
  },
  methods: {
    getMovieDetailes() {
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
          this.movie = response;
        })
        .catch((err) => {
          console.error(err);
        });
      console.log();
    }

  }
};
</script>

<style scoped>
img {
  height: 500px;
}
</style>