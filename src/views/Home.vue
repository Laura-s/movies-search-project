<template>
  <div class="container">
    <div class=""></div>
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

  <div class="container">
    <div class="form-check text-start">
      <input
        v-model="selectat"
        class="form-check-input"
        type="radio"
        name="flexRadioDefault"
        id="flexRadioDefault1"
        value="toate"
      />
      <label class="form-check-label" for="flexRadioDefault1">
        Toate tarile
      </label>
    </div>
    <div class="form-check text-start">
      <input
        v-model="selectat"
        class="form-check-input"
        type="radio"
        name="flexRadioDefault"
        id="flexRadioDefault2"
        value="vecini"
      />
      <label class="form-check-label" for="flexRadioDefault2">
        Vecinii Romaniei
      </label>
    </div>
    <input
      type="text"
      id="input-title-tari"
      class="form-control w-25"
      placeholder="Search"
      aria-label="Search"
      v-model="taraSearch"
    />
    <div class="row">
      <div
        class="col-4 mt-3"
        v-for="(tara, name) in searchResults"
        :key="tara.capitala"
      >
        <div class="card pt-2">
          <h3 class="capitalize">{{ name }}</h3>
          <p class="capitalize">Capitala: {{ tara.capitala }}</p>
          <p>Populatie: {{ parsePopulation(tara.populatie) }}</p>
          <p>Moneda: {{ tara.moneda.toUpperCase() }}</p>
        </div>
      </div>
      <div v-if="Object.keys(searchResults).length == 0">
        <h1>No results</h1>
        <a
          href="#"
          class="text-primary text-decoration-none"
          @click.prevent="taraSearch = ''"
          >Clear search</a
        >
      </div>
    </div>
  </div>
</template>

<script>
import MovieCard from "../components/movie-card";
import filme from "../assets/app.js";
import tari from "../assets/tari.js";
// console.log(filme);
// reduce
// let sum = filme.reduce(function (accumulator, currentValue) {
//     return accumulator + currentValue.pret
// }, 100)
// console.log(sum)

// filme.forEach(e =>{
//   e.disponibil = true
// })

// const filme2 = filme.map(film => {
//   return {
//     nume: film.nume,
//     an: film.an,
//     raiting: film.raiting,
//     pret: film.pret,
//     disponibil : true
//   }
// })
// const filme2 = []
// filme.forEach(film => {
//   const filmNou = {
//     nume: film.nume,
//     an: film.an,
//     raiting: film.raiting,
//     pret: film.pret,
//     disponibil : true
//   }
//   filme2.push(filmNou)
// })

// const filme2 = filme.filter(film => film.an >= 2014)

// const filme2 = [];
// filme.forEach((film) => {
//   if (film.an >= 2014) {
//     filme2.push(film);
//   }
// });

// const filme2 = filme.map(film => {
//   return {
//     nume: film.nume,
//     an: film.an,
//     raiting: film.raiting,
//     pret: film.pret,
//     disponibil : true
//   }
// }).filter(film => film.an >= 2014)

// console.log(filme.every(film => film.raiting == 10))
// console.log(filme.some(film => film.raiting == 10))

// const found = filme.find(element => element.raiting >= 10);
// let result = false
// if(found){
//   result = true
// }

// console.log(result)

// console.log("filme original", filme);
// // console.log("filme 2", filme2);

// console.log(filme.findIndex(element => element.raiting == 5))
Object.values(tari).forEach((tara) => {
  // console.log(tara)
  tara["uniunea europa"] = true;
});

export default {
  data: () => ({
    isLoading: false,
    searchTerm: "",
    movies: [],
    tari: tari,
    selectat: "vecini",
    taraSearch: "",
    titlu: "",
  }),
  computed: {
    results() {
      if (this.selectat == "vecini") {
        //  console.log("cat", Object.values(tari))
        const result = {};
        Object.keys(tari).forEach((tara) => {
          if (
            tari[tara].vecini.includes("România") ||
            tari[tara].vecini.includes("romania")
          ) {
            result[tara] = tari[tara];
          }
        });
        // const vecini = this.tari.romania.vecini
        // vecini.forEach(vecin => {
        //     if(tari[vecin]){
        //       result[vecin] = tari[vecin]
        //     }
        // })
        return result;
      } else {
        return this.tari;
      }
    },
    searchResults() {
      // console.log("primu",this.results)
      const result = {};
      // console.log("nume", Object.keys(this.results));
      Object.keys(this.results).forEach((tara) => {
        if (tara.includes(this.taraSearch)) {
          result[tara] = this.results[tara];
        }
      });

      return result;
    },
  },
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
    parsePopulation(populatie) {
      return Intl.NumberFormat().format(parseInt(populatie) * 1000000);
    },
  },
  components: {
    MovieCard,
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
