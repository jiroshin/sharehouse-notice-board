<template>
  <div id="app">
    <Form></Form>
    <Post></Post>
    <Footer/>
  </div>
</template>

<script>
import Form from '@/components/form.vue'
import Post from '@/components/post.vue'
import Footer from '@/components/footer.vue'
import firebase from 'firebase'

export default {
  name: 'app',
  components: {
    Form,
    Post,
    Footer
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width:80%;
  margin: 50px auto;
}
</style>
