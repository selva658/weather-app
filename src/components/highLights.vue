<template>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Rubik+Glitch&display=swap" rel="stylesheet">
<link href="//netdna.bootstrapcdn.com/font-awesome/3.2.1/css/font-awesome.css" rel="stylesheet">

<div class="rheader">
    <div class="rfcontainer">
       <p class="highlight">Today's Highlight</p>
       <table class="weathertable">
          <tr>
             <td><i class="wi wi-thermometer" id="icon1"></i></td>
             <td class="wname">High</td>
             <td  id="info1">{{hightemp}} °C</td>
             <td><i class="wi wi-thermometer" id="icon2"></i></td>
             <td class="wname">Low</td>
             <td id="info1">{{lowtemp}} °C</td>
          </tr>
          <tr>
             <td><i class="wi wi-humidity" id="icon3"></i></td>
             <td class="wname">Humidity</td>
             <td id="info1">{{humidity}} %</td>
             <td><i class="wi wi-dust" id="icon4"></i></td>
             <td class="wname">Visibility</td>
             <td id="info1">{{visibility}} km</td>
             
          </tr>
           <tr>
             <td><i class="wi wi-horizon-alt" id="icon5"></i></td>
             <td class="wname">UV Index</td>
             <td id="info1">{{uvi}}</td>
             <td><i class="wi wi-strong-wind" id="icon6"></i></td>
             <td class="wname">Wind Speed</td>
             <td id="info1">{{windspeed}} km/h</td> 
          </tr>
           <!-- <tr>
             <td><i class="wi wi-barometer"></i></td>
             <td>Pressure</td>
             <td></td>
             <td><i class="wi wi-raindrops"></i></td>
             <td>Dew Point</td>
             <td></td> 
          </tr> -->
       </table>
    </div>
    <div class="rscontainer">
       <button class="btn1" @click="previous">ᐊ</button>
       <button class="btn" @click="hourlyData">Hourly</button>
      <button class="btn1" @click="next">ᐅ</button>
      <button class="btn" style="margin-left:5%;" @click="dailyData">Daily</button>
       <table  class="weathers" v-if="btnHourly">
          <tr>
             <td><img :src='hourlyicon1'><br>{{h1}}<br>{{h1w}}</td>
             <td><img :src='hourlyicon2'><br>{{h2}}<br>{{h2w}}</td>
             <td><img :src='hourlyicon3'><br>{{h3}}<br>{{h3w}}</td>
             <td><img :src='hourlyicon4'><br>{{h4}}<br>{{h4w}}</td>
          </tr>
       </table>
       <table class="weathers" v-else>
          <tr>
             <td><img :src='dailyicon1'><br>{{d1}}<br>{{d1w}}</td>
             <td><img :src='dailyicon2'><br>{{d2}}<br>{{d2w}}</td>
             <td><img :src='dailyicon3'><br>{{d3}}<br>{{d3w}}</td>
             <td><img :src='dailyicon4'><br>{{d4}}<br>{{d4w}}</td>
          </tr>
       </table>
    </div>
</div>
</template>

<script>
import axios from "axios"
export default{
 data(){
    return {
       highTemp:'',
       lowTemp:'',
       hourlyArr:'',
       hourlyArr1:'',
       hourlyArr2:'',
       hourlyArr3:'',
       btnHourly:true,
       btnDaily:false
    }
 },
props:{
   hightemp:{},
   lowtemp:{},
   latitude:{},
   longitude:{},
   recentlat:{},
   recentlon:{},
   h1:{},
   h2:{},
   h3:{},
   h4:{},
   h1w:{},
   h2w:{},
   h3w:{},
   h4w:{},
   d1:{},
   d2:{},
   d3:{},
   d4:{},
   d1w:{},
   d2w:{},
   d3w:{},
   d4w:{},
   hourlyicon1:{},
   hourlyicon2:{},
   hourlyicon3:{},
   hourlyicon4:{},
   dailyicon1:{},
   dailyicon2:{},
   dailyicon3:{},
   dailyicon4:{},
   humidity:{},
   visibility:{},
   windspeed:{},
   uvi:{},
   previous:{},
   next:{}
},
methods:{
   hourlyData(){
     this.btnHourly=true;
     this.btnDaily=false;

     axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.recentlat==null || ''?this.latitude:this.recentlat}&lon=${this.recentlon==null || ''?this.longitude:this.recentlon}&appid=6fdeade888587f628e2b2739b5d83466`).then((res)=>{
      
         this.hourlyArr=(res.data.hourly[0])
         this.hourlyArr=(res.data.hourly1[1])
         this.hourlyArr=(res.data.hourly2[2])
         this.hourlyArr=(res.data.hourly3[3])
        
     })
   },
   dailyData(){
      this.btnHourly=false;
      this.btnDaily=true;
   }
}
}
</script>


<style scoped>

.rheader{
   height: 60vh;
   width: 40%;
   display: inline-block;
   vertical-align: top;
  
    align-items: center;
   text-align: center ;
   padding:20px;

}

.rfcontainer{
   border:1px solid wheat;
   height: 30vh;
   padding:15px;
   width: 100%;
   align-items: center;
   text-align: center ;
   border-radius: 4%;
   background-color: white;
   padding-bottom: 8%;

   /* background-image: url(https://cdn.pixabay.com/photo/2018/01/14/23/12/nature-3082832__340.jpg); */
   box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px, rgba(0, 0, 0, 0.22) 0px 15px 12px;
}

.rscontainer{
   box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px, rgba(0, 0, 0, 0.22) 0px 15px 12px;
   border:1px solid wheat;
   padding:15px;
   height: 30vh;
   width: 100%;
    padding-bottom: 2%;
   margin-top: 2%;
   text-align: left;
   border-radius: 2%;
   background-color: white;
   /* background-image: url(https://cdn.pixabay.com/photo/2018/01/14/23/12/nature-3082832__340.jpg); */
   box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
}
.btn{
  background-color: blue;
  color: white;
 margin-left:1%;
  height: 18%;
  border:1px solid blue;
  border-radius: 5px;
  cursor: pointer;
  width: 10%;

box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;

    transition: all .2s ease-in-out;

}

.btn:hover{
transform: scale(1.1);

}
.btn1{
 margin-left:1%;
  background-color: blue;
  color: white;
  height: 18%;
  border:1px solid blue;
  border-radius: 5px;
  cursor: pointer;
  width: 5%;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
    transition: all .2s ease-in-out;
}
.btn1:hover{
transform: scale(1.1);

}
/* .rscontainer:nth-child(4){
   margin-left:10%;
} */


.weathers td{
   height: 15vh;
   width: 5%;
   border:1px solid orange;
   text-align: center;
   border-radius: 5px;
   margin-left: 1.2%;
box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;

}
.weathers{
   margin-top: 2%;
 
}
td{ 
  padding: 10px; 
  border-bottom: 1px solid #ccc; 
  text-align: left; 
  font-size: 16px;
  font-family:Arial;
font-weight: 500;
 
  /* color: white; */
  }
td > i {

  font-size: 25px;
}
#icon1{
color: orangered;
}
#icon2{
color: skyblue;
}
#icon3{
color: rgb(11, 211, 144);
}
#icon4{
color: rgb(142, 179, 83);
}
#icon5{
   color:yellow;
}
#icon6{
   color:skyblue;
}
.highlight{
   font-weight: bolder;
   font-weight: bolder;
 font-size: 18px;
  transition: all .2s ease-in-out;
}
.highlight:hover{
transform: scale(1.1);
}
.weathertable{
   margin-left: 4%;
}
#info1{
  transition: all .2s ease-in-out;
}
#info1:hover{
transform: scale(1.1);
}


@media (max-width:500px) {
  .rheader{
     display: block;
     width: 100%;
     padding: 0;
     margin-left: 0;
  }
   td{
     font-size: 14px;
     width: 50px;    
  }
  td > i {
  color: #878686;
  font-size: 14px;
}
  .rscontainer,.rfcontainer{
     height: 30vh;
     padding:0;
     padding-top: 5px;
  }
  .weathers{
     margin-left: 0;
  }
  td{
     width: 2%;
  }

}
@media (min-width:501px) and (max-width:1200px) {
   .rheader{
      display: block;
      width: 100%;
   }
    .rscontainer,.rfcontainer{
     height: 45vh;
     padding:5px;
     /* padding-top: 5px; */
  }
  .weathertable{
     margin: auto;
  }

  .rscontainer{
     text-align: center;
     padding-top: 20px;
     height: 35vh;
  }
}


</style>