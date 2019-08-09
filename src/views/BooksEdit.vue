<template>
  <div class="home">
    <div v-for="error in errors">
      {{error}}
    </div>
    <form v-on:submit.prevent="submit()">
      <p>Name: <input type="text" v-model="book.name"></p>
      <p>Pages: <input type="text" v-model="book.pages"></p>
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
      book: {
        name: "",
        pages:""
      },
      errors: []
    };
  },
  created: function() {
    // get the correct data from the api
    axios.get('/api/books/' + this.$route.params.id).then(response => {
      this.book.name = response.data.name;
      this.book.pages = response.data.pages;
      console.log(response.data);
    })
  },
  methods: {
    submit: function() {
      var params = {
        name: this.book.name,
        pages: this.book.pages
      }
      axios.patch('/api/books/' + this.$route.params.id, params).then(response => {
        this.$router.push('/');
      }).catch(error => {
        console.log('in the .catch')
        console.log(error.response);
        this.errors = error.response.data.errors;
      })
    }
  }
};
</script>
