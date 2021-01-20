<template>
  <div class="col-3 pb-4">
    <div class="card">
      <img
        :src="movie.image && movie.image.url ? movie.image.url : imgNotFoundSrc"
        class="card-img-top"
        alt="..."
      />
      <div class="card-body">
        <h5 class="card-title">
          {{movie.title}}
        </h5>
        <p class="card-text">{{movie.year || "n/a"}}</p>
        <router-link :to="{name: 'Movie', params: {id: movie.id.replace('/title/', '').replace('/', '')}}">More</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import MovieDetailesVue from '../views/MovieDetailes.vue';
export default {
    props: ['movie'],
    data:() =>({
        imgNotFoundSrc : "https://cdn.browshot.com/static/images/not-found.png"
    }),
    created(){
      // console.log(this.movie.id.replace('/title/', '').replace('/', ''))
    }
};

</script>

<style>
</style>