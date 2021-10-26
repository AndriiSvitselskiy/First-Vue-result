<template>
  <div id="app">
    <h1>API</h1>
    <hr />
    <NewCard v-bind:combinedApi="combinedApi" @addCard="addCardToArray"/>
    <hr />
    <Loader v-if="loading" />
    <List
      v-bind:combinedApi="combinedApi"
      v-else-if="combinedApi.length"
      v-on:removeCard="removeCard"
    />
    <p v-else>No cards</p>
  </div>
</template>

<script>
import List from "./components/List";
import Loader from "./components/Loader.vue";
import NewCard from "./components/NewCard.vue";
export default {
  name: "App",
  data() {
    return {
      posts: [],
      photos: [],
      combinedApi: [],
      loading: true,
    };
  },
  async mounted() {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/posts/1/comments"
    );
    const posts = await res.json();
    this.posts = posts;
    const result = await fetch(
      "https://jsonplaceholder.typicode.com/albums/1/photos"
    );
    const photos = await result.json();
    this.photos = photos;

    const combinedArray = this.posts.map((item) => {
      this.photos.forEach(function (i) {
        if (i.id == item.id) {
          item.url = i.url;
        }
      });
      return { item };
    });
    setTimeout(() => {
      this.combinedApi = combinedArray;
      this.loading = false;      
    }, 1000);
  },
  components: {
    List,
    Loader,
    NewCard,
  },
  methods: {
    removeCard(id) {
      this.combinedApi = this.combinedApi.filter(
        (index) => index.item.id !== id
      );
    },
    addCardToArray(item){
      this.combinedApi.push(item)
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
