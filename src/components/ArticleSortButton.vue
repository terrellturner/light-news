<template>
  <buttons :key="category.id" v-for="category in categories">
    <button
      @click.prevent="$emit('pull-data', category.category)"
      :class="shuffleArray(articleTags)[0]"
      href="#"
    >
      {{ category.category }}
    </button>
  </buttons>
</template>

<script>
export default {
  name: "ArticleSortButton",
  setup() {},
  props: {
    categories: Array,
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
  created() {
    this.articleTags = [
      "button is-info is-light",
      "button is-success is-light",
      "button is-warning is-light",
      "button is-danger is-light",
    ];
  },
  emits: ["pull-data"],
};
</script>

<style scoped>
button {
  margin: 0.5rem;
}
</style>
