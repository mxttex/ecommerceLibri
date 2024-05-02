<script>
  import { RouterLink, RouterView } from 'vue-router'
  
</script>

<template>
  <div class="container-fluid" >
    <RouterView @filter="filter"></RouterView>
  </div>
</template>

<style scoped>
  nav{
    align-items: top;
    align-content: top;
  }
  
</style>
