<template>
  <div class="home container">
    <img alt="Vue logo" src="../assets/logo.png">
    <Todo />
  </div>
</template>

<script>
// @ is an alias to /src
import Todo from '@/components/Todo.vue'

export default {
  name: 'Home',
  components: {
    Todo
  }
}
</script>

<style>
  .container {
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
    text-align: left !important;
    background-color: #fff;
    padding: 25px;
    border-radius: 30px;
    border: 3px solid #41B883;
    margin-bottom: 30px;
  }
  
  img {
    display: block;
    margin: 0 auto;
    max-width: 78px;
    margin-bottom: 12px;
    /* background-color: blue; */
  }

  * {
    box-sizing: border-box;
  }
</style>
