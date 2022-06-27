<template>
  <div
    :key="article.index"
    v-for="article in shuffleArray(articles).slice(0, 3)"
    class="column"
  >
    <SingleArticle :article="article" />
  </div>
</template>

//shuffleArray(articles).slice(0, 3)

<script>
import SingleArticle from "./SingleArticle.vue";

export default {
  name: "LightArticles",
  props: {
    articles: Array,
  },
  components: {
    SingleArticle,
  },
  data() {
    return {
      articleArray: [],
    };
  },
  methods: {
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
