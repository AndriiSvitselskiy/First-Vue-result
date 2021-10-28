<template>
  <div class="cardWraper" v-if="visibilityArray.includes(post.item.id)">
    <div>
      <img :src="post.item.url" alt="avatar" />
      <div class="userName">{{ post.item.name | firstLetterUpper }}</div>
      <p>{{ post.item.body | firstLetterUpper }}</p>
    </div>
    <button v-on:click="$emit('remove-card', post.item.id)">delete</button>
  </div>
</template>
// 
<script>
export default {
  props: ["post", "visibilityArray"],
  filters: {
    firstLetterUpper(value) {
      return value[0].toUpperCase() + value.slice(1);
    },
  },
};
</script>
<style scoped>
.onScreen {
  background: red;
}
.cardWraper {
  width: 30%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex-wrap: nowrap;
  align-items: center;
  margin: 1% 1%;
  border: 1px solid black;
}
div {
  width: 90%;
}
.userName {
  width: 100%;
  text-align: left;
  margin-top: 1%;
  font-weight: 600;
  font-size: 30px;
}

p {
  width: 70%;
  text-indent: 20px;
  text-align: left;
  margin: 10px;
  font-size: 20px;
}
button {
  width: 80%;
  margin: 20px;
  font-size: 20px;
  cursor: pointer;
}
img {
  width: 100%;
  height: auto;
  margin: 10px 0;
}
</style>
