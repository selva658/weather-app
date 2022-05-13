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
  
  background-image: url(https://cdn.pixabay.com/photo/2018/01/14/23/12/nature-3082832__340.jpg);
  overflow-x: hidden;

}

</style>