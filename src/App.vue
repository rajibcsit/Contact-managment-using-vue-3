<template>
  
  
  <NavBar />
  <router-view />

</template>

<script>
import NavBar from "./components/NavBar.vue";
//import LoadingComponent from "./components/LoadingComponent.vue";
    export default{
    name: "app",
    components: { NavBar, }
}

</script>

<style>

</style>
