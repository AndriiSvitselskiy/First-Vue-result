<template>
  <form class="newCard" @submit.prevent="onSubmit">
    <h2>Add New card</h2>
    <div>
      <label for="name">Enter your name</label>
      <input type="text" name="name" v-model="name" />
    </div>
    <div>
      <label for="message">Enter your message</label>
      <textarea
        name="message"
        id=""
        cols="30"
        rows="10"
        v-model="body"
      ></textarea>
    </div>
    <button type="submit">Submit</button>
  </form>
</template>
<script>
export default {
  props: ["combinedApi"],
  data() {
    return {
      name: "",
      body: "",
      id: "",
    };
  },
  methods: {
    getRandom(min, max) {
      return (this.id = Math.floor(Math.random() * (max - min) + min));
    },
    onSubmit() {
      this.getRandom(1, 999999);
      if (this.name.trim() && this.body.trim()) {
        for (let i = 0; i < this.combinedApi.length; i++) {
          if (this.id == this.combinedApi[i].item.id) {
            return this.onSubmit();
          }
        }
        const newCard = {
          item: {
            name: this.name,
            body: this.body,
            id: this.id,
            url: "https://img1.freepng.ru/20180624/ivq/kisspng-business-organization-computer-software-tom-clancy-unknown-person-5b2f72c6649235.833799281529836230412.jpg",
            visibility: false,
            centralItem: false,
          },
        };
        this.$emit("addCard", newCard);
        this.name = "";
        this.body = "";
      }
    },
  },
};
</script>
<style scoped>
.newCard {
  width: 80%;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-around;
}
h3 {
  width: 100%;
}
div {
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
label {
  width: 20%;
  font-size: 30px;
}
textarea {
  resize: none;
  margin: 20px 0;
}
textarea,
input {
  width: 50%;
  border-radius: 20px;
  padding: 10px;
  font-size: 20px;
}
button {
  font-size: 30px;
  margin-bottom: 20px;
  width: 15%;
  border-radius: 20px;
}
</style>