<template>
  <div id="app">
  <h1>{{title}}</h1>
  <form  @submit.prevent="findArticles" id="search_form" >
   
    <input type="search" name="search" id="search" v-model="search" >
    <input type="submit" value="Search"  class="button">
   <a :href="random" target="_blank" rel="noopener" id="random" class="button">Random Article</a>
  </form>

  <ul class="article_list">
    <li v-for="article in articles" :key="article.title">
      <div class="article">
      <h2> <a :href="url + article.title" target="_blank" rel="noopener"> {{article.title}}</a></h2>
      <p>{{article.extract}}</p>
     

      <a :href="url + article.title" target="_blank" rel="noopener" class="button"> Read More </a>
    
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

<style lang="scss" >
$color-primary-0: #d3aeae; // Main Primary color */
$color-primary-1: #fff7f7;
$color-primary-2: #fae2e2;
$color-primary-3: #b28181;
$color-primary-4: #935a5a;

#app {
  max-width: 768px;
  margin: 50px auto;
  border: 2px solid $color-primary-0;
  background-color: $color-primary-1;

  border-radius: 5px;
}

.button {
  padding: 6px;
  border: 2px solid $color-primary-4;
  border-radius: 4px;
  background: transparent;
  color: $color-primary-4;
  margin: 10px;
  &:hover {
    color: $color-primary-1;
    background-color: $color-primary-4;
  }
}

#random {
  margin: 20px auto;
  text-align: center;
  display: block;
  width: 120px;
  text-decoration: none;
}

input[type="search"] {
  border-radius: 3px;
  padding: 5px;
  width: 65%;
}
// input[type="search"]:focus {
//   border: 2px solid $color-primary-4;
// }

h1 {
  text-align: center;
  text-transform: uppercase;
  color: $color-primary-4;
}
h2 {
  text-transform: uppercase;
  a {
    color: $color-primary-3;
  }
}

#search_form {
  text-align: center;
}
.article_list {
  padding-left: 0px;

  li {
    list-style: none;
  }
  a {
    display: inline-block;
    text-decoration: none;

    text-align: right;
  }
  .article {
    border: 1px solid $color-primary-4;
    margin: 20px;
    padding: 10px;
    border-radius: 3px;
  }
}
</style>
