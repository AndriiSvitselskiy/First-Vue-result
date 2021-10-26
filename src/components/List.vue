<template>
  <div class="cardWrapper">
    <Item
      li
      v-for="post of combinedApi"
      :key="post.id"
      v-bind:post="post"
      v-on:remove-card="removeCard"
    />
  </div>
</template>

<script>
import Item from "./Item.vue";

export default {
  props: ["combinedApi"],
  name: "List",

  components: {
    Item,
  },
  methods: {
    removeCard(id) {
      this.$emit("removeCard", id);
    },
  },
};
</script>
<style scoped>
.cardWrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
</style>