<template>
  <div class="cardWraper" v-if="visibilityArray.includes(post.item.id)">
    <div>
      <img :src="post.item.url" alt="avatar" />
      <div class="userName">
        <p>{{ post.item.name | firstLetterUpper }}</p>
      </div>
      <p>{{ post.item.body | firstLetterUpper }}</p>
    </div>
    <button v-on:click="$emit('remove-card', post.item.id)">Delete</button>
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
  box-shadow: inset 0 0 20px 1px grey, 0 0 20px 1px grey;
  border-radius: 20px;
}
div {
  width: 100%;
}
.userName {
  width: 100%;
  text-align: left;
  margin-top: 1%;
}

p {
  width: 90%;
  text-indent: 20px;
  text-align: left;
  margin: 10px;
  font-size: 20px;
}
.userName p {
  font-weight: 600;
  font-size: 30px;
}
button {
  width: 80%;
  margin: 20px;
  font-size: 30px;
  cursor: pointer;
  border-radius: 0 0 40px 40px;
  box-shadow: inset 0 0 10px 2px black,0 0 10px 2px black;
  border: 1px solid black;
}
img {
  width: 100%;
  height: auto;
  border-radius: 20px 20px 0 0;
  border-bottom: 1px solid black;
}
</style>
