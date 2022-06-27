<template>
  <div class="card">
    <div class="card-content">
      <a class="title" :href="article.url">
        {{ article.title }}
      </a>
      <div class="image content is-16by9">
        <img style="object-fit: cover" :src="article.image_url" alt="" />
      </div>
      <p class="subtitle">
        {{ article.source }}
      </p>
    </div>
    <footer class="card-footer">
      <p class="card-footer-item">
        <span>
          <a
            :href="
              'https://www.facebook.com/sharer/sharer.php?u=' + article.url
            "
          >
            <i class="fab fa-facebook"></i
          ></a>
        </span>
      </p>
      <p class="card-footer-item">
        <span>
          <a :href="'https://twitter.com/intent/tweet?text=' + article.url"
            ><i class="fab fa-twitter"></i
          ></a>
        </span>
      </p>
    </footer>
  </div>
</template>

<script>
export default {
  name: "SingleArticle",
  components: {},
  props: {
    article: Object,
  },
  setup() {},
};
</script>

<style scoped>
a {
  font-size: 1.5rem;
}
.card {
  animation: fadeInAnimation ease 3s;
  animation-iteration-count: 1;
  animation-fill-mode: forwards;
}

@keyframes fadeInAnimation {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.card-content > .subtitle {
  font-size: 1rem;
}

.card > .card-content > .title {
  padding-bottom: -10px;
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}

@media screen and (max-width: 768px) {
  .card-content {
    max-height: 100%;
    margin-bottom: 0;
  }
}

@media screen and (min-width: 1680px) {
  .card > .card-content > .title {
    height: 3.6rem;
  }
}
</style>
