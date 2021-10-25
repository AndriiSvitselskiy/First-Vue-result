<template>
  <div id="app">
    <h1>API</h1>
    <hr />
    <List v-bind:combinedApi="combinedApi" />
  </div>
</template>

<script>
import List from "./components/List";

export default {
  name: "App",
  data() {
    return {
      posts: [],
      photos: [],
      combinedApi: [],
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
          console.log("popalsya");
          item.url = i.url;
        }
      });
      console.log(item.id);
      return { item };
    });
this.combinedApi=combinedArray
    // console.log(this.combinedApi);
    // console.log(this.posts);
  },
  components: {
    List,
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
