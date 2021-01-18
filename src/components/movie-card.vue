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
      </div>
    </div>
  </div>
</template>

<script>
export default {
    props: ['movie'],
    data:() =>({
        imgNotFoundSrc : "https://cdn.browshot.com/static/images/not-found.png"
    })
};
</script>

<style>
</style>