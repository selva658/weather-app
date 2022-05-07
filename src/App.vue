<template>
  <div>
    <Search></Search>
    <!-- <Highlight></Highlight> -->
  </div>
</template>


<script>
import Search from "./components/searchWeather.vue"
//import Highlight from "./components/highLights.vue"

export default{
components:{
  Search,
  // Highlight
},

}
</script>

<style>
body{
  text-align: center;
  background-color: rgb(185, 72, 30);
  /* background-image: url(https://cdn.pixabay.com/photo/2021/04/26/01/39/trees-6207925__340.jpg); */
  overflow-x: hidden;

}

</style>