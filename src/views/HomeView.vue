<script setup>
import TheWelcome from '../components/TheWelcome.vue'
</script>

<template>
  <div class="container">
    <h1>web toko buku - andre</h1>
  </div>
</template>

<style>
  .container{
    padding-top: 50%;
  }
</style>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      posts: [],
      errors: []
    }
  },
  created() {
    // axios.get(`http://jsonplaceholder.typicode.com/posts`)
    axios.get('http://andrejohan.my.id/backend-uas/')
    .then(response => {
      // JSON responses are automatically parsed.
      this.posts = response.data
      console.log(this.posts)
    })
    .catch(e => {
      this.errors.push(e)
    })
  }
}
</script>
