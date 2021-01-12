<template>
  <div class="container">
  <div class="row justify-content-center py-5" id="search-row">
    <div class="col-5">
      <div class="input-group ">
        <input type="text" id="input-title" class="form-control" placeholder="Title" aria-label="Title" aria-describedby="button-addon2" @keyup="buttonReact" ref="input" @keydown="getResults">
        <button class="btn btn-outline-primary disabled" type="button" id="button-addon2" ref="button" @click="getResults">
          <span class="spinner-border spinner-border-sm " role="status" aria-hidden="true"></span>
          Search</button>
      </div>
    </div>
  </div>
  <div class="row" id="results-row" ref="row"></div>
</div>
</template>

<script>
export default {
  methods: {
    getResults(){
      const url = 'https://imdb8.p.rapidapi.com/title/find?q='+ this.$refs.input.value.trim();
      const options = {
        "method": "GET",
        "headers": {
            "x-rapidapi-key": "4e037fb5a4mshd017cdd280e6974p11ccbdjsna16daae232bc",
            "x-rapidapi-host": "imdb8.p.rapidapi.com"
        }
      }
      this.$refs.button.classList.add('loading');

      fetch(url, options)
      .then(response => response.json())
      .then(data => {
        this.$refs.button.classList.remove('loading');

        this.$refs.row.innerHTML = ''

        data.results.filter(movie => movie.titleType == "movie").forEach( movie => {
          const imgNotFoundSrc = 'https://cdn.browshot.com/static/images/not-found.png';
          const imgSrc = movie.image && movie.image.url ? movie.image.url : imgNotFoundSrc;
          const movieYear = movie.year || 'n/a';
          const colHtml = `
            <div class="card">
                <img src="${imgSrc}" class="card-img-top" alt="...">
                <div class="card-body">
                <h5 class="card-title">${movie.title}</h5>
                <p class="card-text">${movieYear}</p>
                </div>
            </div>`
        
          const colon = document.createElement('DIV')
          colon.innerHTML = colHtml;
          colon.classList.add('col-3', 'pb-4');
          this.$refs.row.appendChild(colon);
        });
        
      });
    },

    buttonReact(e){
      if (e.target.value.trim() !== ""){
        this.$refs.button.classList.remove('disabled')
      }else{
        this.$refs.button.classList.add('disabled')
      }
    }
  }
}
</script>

<style>
#search-row .spinner-border{
  display: none;
}

#search-row .loading .spinner-border{
  display: inline-block;

}

.card-body .card-title{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.card img{
  height: 300px;
  object-fit: cover;
}
#button-addon2{
  width: 100px;
}
</style>
