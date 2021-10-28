<template>
  <div class="listWraper">
    <img :src="leftArrow" alt="leftArrow" v-on:click="decreaseCard" />
    <div class="cardWrapper">
      <Item
        v-for="post of combinedApi"
        :key="post.id"
        v-bind:post="post"
        v-bind:visibilityArray="visibilityArray"
        v-on:remove-card="removeCard"
      />
    </div>
    <img :src="rightArrow" alt="leftArrow" v-on:click="increaseCard" />
  </div>
</template>
<script>
import Item from "./Item.vue";

export default {
  props: ["combinedApi"],
  name: "List",
  data() {
    return {
      visibilityArray: [],
      removed: [],

      leftArrow: "https://cdn-icons-png.flaticon.com/512/54/54782.png",
      rightArrow: "https://cdn-icons-png.flaticon.com/512/271/271228.png",
    };
  },
  mounted() {
    for (let i = 0; i < this.combinedApi.length; i++) {
      if (this.combinedApi[i].item.centralItem == true) {
        this.combinedApi[i].item.visibility = true;
        this.combinedApi[i + 1].item.visibility = true;
        this.combinedApi[i + 2].item.visibility = true;
      }
    }
    this.pullArray();
  },
  components: {
    Item,
  },
  watch: {
    combinedApi() {
      this.pullArray();
    },
  },

  methods: {
    removeCard(id) {
      this.$emit("removeCard", id);
      for (let i = 0; i < this.combinedApi.length; i++) {
        if (this.combinedApi[i].item.id == id) {
          this.visibilityForRemove(i);
        }
      }
    },
    visibilityForRemove(i) {
      if (this.combinedApi[i].item.centralItem == true) {
        if (this.combinedApi.length > 3) {
          if (this.combinedApi[i + 3] !== undefined) {
            this.combinedApi[i + 1].item.centralItem = true;
            this.combinedApi[i + 1].item.visibility = true;
            this.combinedApi[i + 2].item.visibility = true;
            this.combinedApi[i + 3].item.visibility = true;
            this.pullArray();
          } else if (this.combinedApi[i + 2] !== undefined) {
            this.combinedApi[i - 1].item.centralItem = true;
            this.combinedApi[i - 1].item.visibility = true;
            this.combinedApi[i + 1].item.visibility = true;
            this.combinedApi[i + 2].item.visibility = true;
            this.pullArray();
          } else if (this.combinedApi[i + 1] !== undefined) {
            this.combinedApi[i - 2].item.centralItem = true;
            this.combinedApi[i - 2].item.visibility = true;
            this.combinedApi[i - 1].item.visibility = true;
            this.combinedApi[i + 1].item.visibility = true;
            this.pullArray();
          }
        } else if (this.combinedApi.length == 3) {
          this.combinedApi[i + 1].item.centralItem = true;
          this.combinedApi[i + 1].item.visibility = true;
          this.combinedApi[i + 2].item.visibility = true;
          this.pullArray();
        } else if (this.combinedApi.length == 2) {
          this.combinedApi[i + 1].item.centralItem = true;
          this.combinedApi[i + 1].item.visibility = true;
          this.pullArray();
        } else if (this.combinedApi.length == 1) {
          this.visibilityArray = [];
        }
      } else if (this.combinedApi[i - 1].item.centralItem == true) {
        if (this.combinedApi.length > 3) {
          if (this.combinedApi[i + 2] !== undefined) {
            this.combinedApi[i - 1].item.visibility = true;
            this.combinedApi[i + 1].item.visibility = true;
            this.combinedApi[i + 2].item.visibility = true;
            this.pullArray();
          } else if (this.combinedApi[i + 2] == undefined) {
            this.combinedApi[i - 1].item.centralItem = false;
            this.combinedApi[i - 2].item.centralItem = true;
            this.combinedApi[i + 1].item.visibility = true;
            this.combinedApi[i - 1].item.visibility = true;
            this.combinedApi[i - 2].item.visibility = true;
            this.pullArray();
          }
        }
      } else if (this.combinedApi[i - 2].item.centralItem == true) {
        if (this.combinedApi.length > 3) {
          if (this.combinedApi[i + 1] !== undefined) {
            this.combinedApi[i - 2].item.visibility = true;
            this.combinedApi[i - 1].item.visibility = true;
            this.combinedApi[i + 1].item.visibility = true;
            this.pullArray();
          } else return;
        }
      }
    },
    pullArray() {
      this.visibilityArray = [];
      if (this.combinedApi.length == 1) {
        this.combinedApi[0].item.centralItem = true;
      }
      this.combinedApi.forEach((i) => {
        if (i.item.visibility == true) {
          this.visibilityArray.push(i.item.id);
        }
      });
    },
    upForOne(i) {
      if (this.combinedApi.length > 3) {
        if (
          this.combinedApi[i].item.centralItem == true &&
          i < this.combinedApi.length - 2
        ) {
          this.combinedApi[i].item.centralItem = false;
          this.combinedApi[i + 1].item.centralItem = true;
          this.combinedApi[i + 1].item.visibility = true;
          this.combinedApi[i + 2].item.visibility = true;
          this.combinedApi[i + 3].item.visibility = true;
          this.pullArray();
        } else if (this.combinedApi[i - 1].item.centralItem == true) {
          this.combinedApi[i + 2].item.visibility = true;
          this.pullArray();
        } else if (this.combinedApi[i - 2].item.centralItem == true) {
          this.combinedApi[i + 1].item.visibility = true;
          this.pullArray();
        }
      } else if (this.combinedApi.length == 3) {
        return;
      }
    },
    decreaseForOne(i) {
      if (this.combinedApi[i].item.centralItem == true) {
        this.combinedApi[i].item.centralItem = false;
        this.combinedApi[i - 1].item.centralItem = true;
        this.combinedApi[i - 1].item.visibility = true;
        this.combinedApi[i].item.visibility = true;
        this.combinedApi[i + 1].item.visibility = true;
        this.pullArray();
      }
    },
    decreaseCard() {
      this.combinedApi.forEach((index) => {
        index.item.visibility = false;
      });
      for (let i = 0; i < this.combinedApi.length; i++) {
        if (this.combinedApi[i].item.centralItem == true) {
          if (i < 1) {
            return;
          } else {
            return this.decreaseForOne(i);
          }
        }
      }
    },
    increaseCard() {
      this.combinedApi.forEach((index) => {
        index.item.visibility = false;
      });
      for (let i = 0; i < this.combinedApi.length; i++) {
        if (this.combinedApi[i].item.centralItem == true) {
          if (i > this.combinedApi.length - 4) {
            return;
          } else {
            return this.upForOne(i);
          }
        }
      }
    },
  },
};
</script>
<style scoped>
.listWraper {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.cardWrapper {
  width: 90%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
#leftCanvas {
  border: 1px solid black;
}
img {
  width: 5%;
  cursor: pointer;
}
</style>