<script setup>
defineProps(['login', 'name', 'company', 'bio', 'avatar_url'])
</script>

<template>
    <img :src="avatar_url">
    <!-- : == v-bind -->
    <strong>@{{login}} </strong> 
    <h1> {{name}} </h1>
    <h2> {{company}} </h2>
    <span> {{bio}} </span>
</template>

<style scoped>
img {
    border: 1px solid #e5e5e5;
    border-radius: 999px;
    display: block;
    margin: 1rem auto;
    width: 8rem;
    height: 8rem;
  }
  
  h1, h2 {
    color: #f64348;
    margin: 1rem auto .25rem;
  }
  
  h3 {
    margin: 1rem auto .25rem;
  }
  
  span{
    display: block;
    margin: 1rem auto;
  }
</style>