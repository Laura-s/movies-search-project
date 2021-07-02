<template>
  <div class="col-sm-12 col-md-4 col-lg-3  pb-4">
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
export default {
    
    props: ['movie'],
    data:() =>({
        imgNotFoundSrc : "https://cdn.browshot.com/static/images/not-found.png"
    }),
    
    created(){
      // console.log(this.movie.id.replace('/title/', '').replace('/', ''))
      // console.log(`Injected property: ${this.titlu}`)
    },
    methods:{
      // trimitetitlu(){
      //   this.$emit("trimitetitlu", this.movie.title);
      //     console.log('message emit from child component')
      // bum
      // }
    }
};

</script>

<style>
</style>