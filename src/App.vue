<template>
  <section class="hero is-primary is-medium">
    <div class="hero-head">
      <nav class="navbar is-primary">
        <div class="container">
          <div class="navbar-brand">
            <a href="#" class="navbar-item">LightNews</a>
            <span class="navbar-burger" data-target="navbarIndex">
              <span></span>
              <span></span>
              <span></span>
            </span>
          </div>
          <div class="navbar-menu" id="navbarIndex">
            <div class="navbar-end">
              <a href="index.html" class="navbar-item">Home</a>
              <a href="#" class="navbar-item">About</a>
              <span class="navbar-item">
                <a href="" class="button is-danger is-inverted"
                  ><span class="icon">
                    <i class="fa-brands fa-github"></i>
                  </span>
                  <span>Repo</span>
                </a>
              </span>
            </div>
          </div>
        </div>
      </nav>
    </div>
    <div class="hero-body">
      <div class="container has-text-centered">
        <div class="title">
          <VueWriter
            :array="['LightNews.']"
            :iterations="1"
            :caret="underscore"
          />
        </div>
        <p class="subtitle">Relevant news, fast and lightweight.</p>
      </div>
    </div>
  </section>
  <div class="container is-fluid">
    <div class="content has-text-centered">
      <div class="content">
        <ArticleSortButton
          @pull-data="getNewArticles"
          :categories="categories"
        />
      </div>
    </div>
    <div class="section">
      <div
        :key="article.index"
        v-for="article in articles"
        class="columns is-desktop"
      >
        <LightArticles :articles="articles" />
      </div>
    </div>
  </div>
</template>

<script>
import LightArticles from "./components/LightArticles.vue";
import ArticleSortButton from "./components/ArticleSortButton.vue";
import VueWriter from "vue-writer";

export default {
  name: "App",
  components: { LightArticles, ArticleSortButton, VueWriter },
  data() {
    return {
      articles: [],
      articleArray: [],
      categories: [],
    };
  },
  created() {
    this.categories = [
      { id: 0, category: "general" },
      { id: 1, category: "science" },
      { id: 2, category: "sports" },
      { id: 3, category: "business" },
      { id: 4, category: "health" },
      { id: 5, category: "entertainment" },
      { id: 6, category: "tech" },
      { id: 7, category: "politics" },
      { id: 8, category: "food" },
      { id: 9, category: "travel" },
    ];
  },
  methods: {
    async retrieveArticles(category = "general") {
      console.log(category);
      const cacheData = sessionStorage.getItem("site-data");
      var url =
        "https://api.thenewsapi.com/v1/news/top?api_token=PtqU8VXn2wmMHIZa9MYNj9Po6vX4tdl5UaoQo9tY&locale=us" +
        `&categories=${category}`;
      var req = new Request(url);

      if (cacheData !== null) {
        const dataParsed = await JSON.parse(
          sessionStorage.getItem("site-data")
        );
        this.articles = dataParsed.data;
      } else {
        const rawData = await fetch(req);
        const data = await rawData.json();

        sessionStorage.setItem("site-data", await JSON.stringify(data));

        const dataParsed = await JSON.parse(
          sessionStorage.getItem("site-data")
        );
        this.articles = dataParsed.data;
      }
    },
    getNewArticles(category) {
      console.log(category);
      sessionStorage.removeItem("site-data");
      this.retrieveArticles(category);
    },
  },
  mounted() {
    this.retrieveArticles();
  },
  computed: {},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@1,6..72,300&display=swap");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
h1 {
  font-size: 3rem;
}
p.title {
  font-size: 1.6rem;
}
.content {
  margin-top: 1rem;
}
.content a {
  margin-right: 0.1rem;
}

.hero-body {
  background-image: url("../public/imgs/light.jpg");
}

.hero-body > .container {
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(5px);
  padding: 5rem;
}

.is-typed {
  font-family: "Newsreader", serif;
}

.is-typed span.typed {
  color: rgba(255, 255, 255);
}

.is-typed span.cursor {
  display: inline-block;
  width: 3px;
  background-color: rgb(255, 255, 255);
  animation: blink 1s infinite;
}

.is-typed span.underscore {
  display: inline-flex;
  width: 10px;
  height: 1px;
  align-items: flex-end;
  background-color: rgb(255, 255, 255);
  animation: blink 1s;
}

.is-typed span.cursor.typing {
  animation: none;
}

@keyframes blink {
  49% {
    background-color: rgb(255, 255, 255);
  }
  50% {
    background-color: transparent;
  }
  99% {
    background-color: transparent;
  }
}
</style>
