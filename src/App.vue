<template>
  <div id="apps">
  <h1>{{title}}</h1>
    <a :href="random" target="_blank" rel="noopener"><input type="button" value="Random"></a>
  <form  @submit.prevent="findArticles">
    <input type="search" name="search" id="search" v-model="search">
    <input type="submit" value="Search" >
  </form>
  <ul>
    <li v-for="article in articles" :key="article.title">
      <div>
      <p><a :href="url + article.title" target="_blank" rel="noopener">  {{article.title}}</a></p>
      <p>{{article.extract}}</p>
    </div>
   </li>
  </ul>
  </div>
   
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      title: "wikipedia viewer",
      search: "",
      articles: [],
      url: "http://en.wikipedia.org/wiki/",
      random: "https://en.wikipedia.org/wiki/Special:Random"
    };
  },
  methods: {
    findArticles() {
      const endpoint = `https://en.wikipedia.org/w/api.php?format=json&action=query&title=<query>&generator=search&limit=10&prop=pageimages|extracts&pilimit=max&exintro&explaintext&exsentences=1&origin=*&exlimit=max&gsrsearch=${this
        .search}`;
      fetch(endpoint)
        .then(data => data.json())
        .then(data => {
          this.articles = data.query.pages;
          console.log(this.articles);
        });
    }
  }
};
</script>

<style>

</style>
