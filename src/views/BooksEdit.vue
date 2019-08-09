<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <form v-on:submit.prevent="submit()">
      <p>Name: <input type="text" v-model="bookName"></p>
      <p>Pages: <input type="text" v-model="bookPages"></p>
      <input type="submit" value="Edit this book">
    </form>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      message: "Welcome to Edit page!",
      bookName: "",
      bookPages: ""
    };
  },
  created: function() {
    // get the correct data from the api
    axios.get('/api/books/' + this.$route.params.id).then(response => {
      this.bookName = response.data.name;
      this.bookPages = response.data.pages;
      console.log(response.data);
    })
  },
  methods: {
    submit: function() {
      var params = {
        name: this.bookName,
        pages: this.bookPages
      }
      axios.patch('/api/books/' + this.$route.params.id, params).then(response => {
        this.$router.push('/');
      })
    }
  }
};
</script>
