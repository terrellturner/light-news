<template>
  <section class="hero is-medium">
    <div class="hero-head">
      <nav class="navbar is-transparent">
        <div class="navbar-brand">
          <a href="#" id="logo-text" class="navbar-item">LN.</a>
          <span
            class="navbar-burger"
            :class="{ 'is-active': showNavBar }"
            @click="showNavBar = !showNavBar"
            data-target="navbarIndex"
          >
            <span></span>
            <span></span>
            <span></span>
          </span>
        </div>
        <div
          class="navbar-menu"
          :class="{ 'is-active': showNavBar }"
          id="navbarIndex"
        >
          <div class="navbar-end">
            <a href="index.html" class="navbar-item">Home</a>
            <a href="#" class="navbar-item">About</a>
            <span class="navbar-item">
              <a href="#" class="button is-danger is-inverted"
                ><span class="icon">
                  <i class="fa-brands fa-github"></i>
                </span>
                <span>Repo</span>
              </a>
            </span>
          </div>
        </div>
      </nav>
    </div>
    <div class="hero-body">
      <div class="box has-text-centered">
        <div class="title">
          <VueWriter :array="['LightNews.']" :iterations="1" />
        </div>
        <p class="subtitle">Relevant news, fast and lightweight.</p>
      </div>
    </div>
  </section>
  <div class="container is-fluid">
    <div class="content has-text-centered">
      <div id="button-container" class="content">
        <ArticleSortButton
          @pull-data="getNewArticles"
          :categories="categories"
          @click="fadeOut = !fadeOut"
        />
      </div>
    </div>
    <div class="section">
      <div class="columns is-centered is-multiline">
        <LightArticles :articles="articles" :fadeOut="fadeOut" />
      </div>
    </div>
  </div>
  <footer class="footer">
    <div
      class="container has-text-centered"
      :set="(currentYear = new Date().getFullYear())"
    >
      <p>&copy; {{ currentYear }} rellSoft</p>
    </div>
  </footer>
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
      showNavBar: false,
      fadeOut: false,
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
      this.handleArticleFade();

      const cacheData = sessionStorage.getItem("site-data");
      const api = process.env.API_KEY;
      var url =
        `https://api.thenewsapi.com/v1/news/all?language=en&locale=us&api_token=${api}` +
        `&categories=${category}` +
        `&exclude_domains=*.medium.com,*.foxnews.com,video.foxnews.com,medium.com`;
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

        this.fadeOut = !this.fadeOut;
        console.log(this.fadeOut);
      }
    },
    getNewArticles(category) {
      this.handleArticleFade();
      console.log(this.fadeOut);
      sessionStorage.removeItem("site-data");
      this.retrieveArticles(category);
    },
    storageCheck(key) {
      if (!sessionStorage.getItem(key)) {
        return true;
      }
    },
    handleArticleFade() {
      if (this.storageCheck("site-data")) {
        this.fadeOut = !this.fadeOut;
      }
    },
  },

  //Run at startup
  mounted() {
    this.retrieveArticles();
  },
  computed: {},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,300;1,6..72,300;1,6..72,400;1,6..72,700&family=Poppins:ital,wght@0,300;0,700;1,100&display=swap");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#button-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
}

#logo-text {
  font-size: 2rem;
  padding-top: 1rem;
  color: #fff;
}

h1 {
  font-size: 3rem;
}
p.title {
  font-size: 1.6rem;
}
.subtitle {
  font-size: 0.5rem;
  color: white;
}
.content {
  margin-top: 1rem;
}
.content a {
  margin-right: 0.1rem;
}

.navbar {
}

.navbar-brand,
.navbar-menu {
  backdrop-filter: blur(5px);
}

.navbar-menu {
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(5px);
  text-align: center;
}

.navbar-burger > span {
  color: #fff;
}

.navbar-item {
  color: #fff;
}

.navbar-end > .navbar-item:hover {
  background-color: rgba(255, 255, 255, 0);
  color: #fff;
}

.hero-head {
  width: 100%;
}

.hero-body {
  background-image: url("../public/imgs/light.jpg");
  position: relative;
  display: flex;
  justify-content: center;
  padding: 4rem;
}

.hero-head {
  position: absolute;
  width: 100%;
}

.hero-body > .box {
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(5px);
  padding: 2rem;
  width: 50%;
  margin-top: 2rem;
}

.is-typed,
#logo-text {
  font-family: "Newsreader", serif;
  font-style: italic;
  font-weight: lighter;
}

.is-typed span.typed {
  color: rgba(255, 255, 255);
}

.is-typed span.cursor {
  display: inline-block;
  width: 2px;
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

.container > .navbar-brand > .navbar-burger {
  margin-top: auto;
  margin-bottom: auto;
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

@media screen and (max-width: 1024px) {
  #logo-text {
    font-size: 1rem;
  }
  .hero-body {
    padding: 0;
  }
  .hero-body > .box {
    margin-top: 3rem;
    width: 100%;
  }
  .container.is-fluid,
  .section {
    padding-left: 0;
    padding-right: 0;
  }
}
</style>
