<template>
  <div
    :key="article.index"
    v-for="article in articles"
    class="column is-one-third"
  >
    <SingleArticle
      :key="article.uuid"
      :article="article"
      :fadeOut="fadeOut"
      :class="{ 'fade-out-transition': fadeOut }"
    />
  </div>
</template>

//shuffleArray(articles).slice(0, 3)

<script>
import SingleArticle from "./SingleArticle.vue";

export default {
  name: "LightArticles",
  props: {
    articles: Array,
    fadeOut: Boolean,
  },
  components: {
    SingleArticle,
  },
  data() {
    return {
      articleArray: [],
      fadeOutTransition: false,
    };
  },
  mounted() {
    this.articleArray = this.shuffleArray(this.articles);
  },
  methods: {
    randomNum(min = 0, max = 1) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
  },
};
</script>

<style scoped></style>
