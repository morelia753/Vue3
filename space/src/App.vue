<template>
  <NavBar/>
  <router-view :key="$route.fullPath"/>
</template>

<script>
  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap/dist/js/bootstrap.js';

  import NavBar from './components/NavBar.vue';

  export default {
    name : "APP",
    components : {
      NavBar,
    }
  }
  
</script>

