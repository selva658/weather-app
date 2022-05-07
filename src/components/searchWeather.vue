<template>
<h1 class="h1">Weather App</h1>
    <div class="lcontainer">
       <input v-model="inpvalue" v-on:keyup.enter="userSearch"/>
       <button @click="userSearch" class="btn" >Search</button>
       <br>
       <img :src='currenticon' class="curr-icon">
       <br>
       <h1 class="loc">{{curLocationName}}</h1>
       <p class="loc">,{{timezone}}</p>
       <p class="curr-time">{{currentTime}}</p>
       <h1 class="temp">{{temperature}}°</h1>
       <p class="weather">{{weather}}</p>
       <p>{{curLocationName}} feels like:{{feelsLike}}°</p>
       <p class="sun">🔆{{sunrise}}AM</p>
       <p class="sun">🔅{{sunset}}PM</p>
       
    </div>
     <high-light
     :hightemp='highTemp' 
     :lowtemp='lowTemp' 
     :latitude='latitude' 
     :longitude='longitude' 
     :recentlat='recentLatitude' 
     :recentlon='recentLongitude' 
     :h1='hourlyArr' 
     :h2='hourlyArr1' 
     :h3='hourlyArr2' 
     :h4='hourlyArr3'
     :h1w='hourlyWeather1'
     :h2w='hourlyWeather2'
     :h3w='hourlyWeather3'
     :h4w='hourlyWeather4'
     :d1='dailyArr'
     :d2='dailyArr1'
     :d3='dailyArr2'
     :d4='dailyArr3'
     :d1w='dailyWeather1'
     :d2w='dailyWeather2'
     :d3w='dailyWeather3'
     :d4w='dailyWeather4'
     :hourlyicon1='hourlyicon1'
     :hourlyicon2='hourlyicon2'
     :hourlyicon3='hourlyicon3'
     :hourlyicon4='hourlyicon4'
     :dailyicon1='dailyicon1'
     :dailyicon2='dailyicon2'
     :dailyicon3='dailyicon3'
     :dailyicon4='dailyicon4'
     :humidity='humidity'
     :visibility='visibility'
     :uvi='uvi'
     :windspeed='windspeed'
     >
     </high-light>
</template>

<script>
import axios from "axios"
import HighLight from "./highLights.vue"
export default{
    data(){
        return{
            latitude:'',
            longitude:'',
            curLocationName:'',
            timezone:'',
            currentTime:'',
            temperature:'',
            weather:'',
            feelsLike:'',
            sunrise:'',
            sunset:'',
            highTemp:'',
            lowTemp:'',
            inpvalue:'',
            recentLatitude:JSON.parse(localStorage.getItem("latitude")),
            recentLongitude:JSON.parse(localStorage.getItem("longitude")),
            hourlyArr:'',
            hourlyWeather1:'',
            hourlyArr1:'',
            hourlyWeather2:'',
            hourlyArr2:'',
            hourlyWeather3:'',
            hourlyArr3:'',
            hourlyWeather4:'',
            dailyArr:'',
            dailyArr1:'',
            dailyArr2:'',
            dailyArr3:'',
            dailyWeather1:'',
            dailyWeather2:'',
            dailyWeather3:'',
            dailyWeather4:'',
            hourlyicon1:'',
            hourlyicon2:'',
            hourlyicon3:'',
            hourlyicon4:'',
            dailyicon1:'',
            dailyicon2:'',
            dailyicon3:'',
            dailyicon4:'',
            currenticon:'',
            compmount:false,
            humidity:'',
            visibility:'',
            uvi:'',
            windspeed:''
            
        }
    },
    components:{
      HighLight
    },
    methods:{
        trackMyLocation(){
            if(navigator.geolocation){
             navigator.geolocation.getCurrentPosition(position=>{
                 this.latitude=position.coords.latitude
                 this.longitude=position.coords.longitude
                 this.location()
             },error=>{
                 console.log(error)
             })
            }
            else{
                console.log("Not Support")
            }
        },
        location(){

         if(this.recentLatitude===null || this.recentLongitude===null){
         //GET request for weather and all
         axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.latitude}&lon=${this.longitude}&appid=6fdeade888587f628e2b2739b5d83466&units=metric`).then((res)=>{
         //console.log(res.data);


         //currenticon(mainicon)
         let cur_icon=res.data.current.weather[0].icon
         this.currenticon=`https://openweathermap.org/img/w/${cur_icon}.png`

         //timezone
         this.timezone=res.data.timezone

         //for getting current time
         let unix=(res.data.current.dt)
         let dat=new Date(unix*1000)
         this.currentTime=dat

         //temperature
         this.temperature=Math.ceil(res.data.current.temp)

         //weather
         this.weather=res.data.current.weather[0].main

         //feelslike
         this.feelsLike=Math.ceil(res.data.current.feels_like)

         //sunrise
         let unix1=(res.data.current.sunrise)
         let dat1=new Date(unix1*1000)
         this.sunrise=dat1.toLocaleTimeString();
        
         //sunset
         let unix2=(res.data.current.sunset)
         let dat2=new Date(unix2*1000)
         this.sunset=dat2.toLocaleTimeString();
        
       //hourly-Data
         let hour1=(res.data.hourly[0].dt)
         let hour11=new Date(hour1*1000)
         this.hourlyArr=hour11.toLocaleTimeString()

         let hour2=(res.data.hourly[1].dt)
         let hour22=new Date(hour2*1000)
         this.hourlyArr1=hour22.toLocaleTimeString()

         let hour3=(res.data.hourly[2].dt)
         let hour33=new Date(hour3*1000)
         this.hourlyArr2=hour33.toLocaleTimeString()

         let hour4=(res.data.hourly[3].dt)
         let hour44=new Date(hour4*1000)
         this.hourlyArr3=hour44.toLocaleTimeString()

        //hourly-weather
         this.hourlyWeather1=res.data.hourly[0].weather[0].main
         this.hourlyWeather2=res.data.hourly[1].weather[0].main
         this.hourlyWeather3=res.data.hourly[2].weather[0].main
         this.hourlyWeather4=res.data.hourly[3].weather[0].main

        
        //hourly-weather images
         let icon1=res.data.hourly[0].weather[0].icon
         this.hourlyicon1=`https://openweathermap.org/img/w/${icon1}.png`;

         let icon2=res.data.hourly[1].weather[0].icon
         this.hourlyicon2=`https://openweathermap.org/img/w/${icon2}.png`;

         let icon3=res.data.hourly[2].weather[0].icon
         this.hourlyicon3=`https://openweathermap.org/img/w/${icon3}.png`;

         let icon4=res.data.hourly[3].weather[0].icon
         this.hourlyicon4=`https://openweathermap.org/img/w/${icon4}.png`;

        //Daily-data
         let daily1=(res.data.daily[0].dt)
         let daily11=new Date(daily1*1000)
         this.dailyArr=daily11.toLocaleDateString()

         let daily2=(res.data.daily[1].dt)
         let daily22=new Date(daily2*1000)
         this.dailyArr1=daily22.toLocaleDateString()

         let daily3=(res.data.daily[2].dt)
         let daily33=new Date(daily3*1000)
         this.dailyArr2=daily33.toLocaleDateString()

         let daily4=(res.data.daily[3].dt)
         let daily44=new Date(daily4*1000)
         this.dailyArr3=daily44.toLocaleDateString()

        //daily-weather
         this.dailyWeather1=res.data.daily[0].weather[0].main
         this.dailyWeather2=res.data.daily[2].weather[0].main
         this.dailyWeather3=res.data.daily[2].weather[0].main
         this.dailyWeather4=res.data.daily[2].weather[0].main
        // console.log(this.dailyWeather1)

        //daily-weather images
         let dicon1=res.data.daily[0].weather[0].icon
         this.dailyicon1=`https://openweathermap.org/img/w/${dicon1}.png`;

         let dicon2=res.data.daily[1].weather[0].icon
         this.dailyicon2=`https://openweathermap.org/img/w/${dicon2}.png`;

         let dicon3=res.data.daily[2].weather[0].icon
         this.dailyicon3=`https://openweathermap.org/img/w/${dicon3}.png`;

         let dicon4=res.data.daily[3].weather[0].icon
         this.dailyicon4=`https://openweathermap.org/img/w/${dicon4}.png`;


        //humdity
        this.humidity=res.data.current.humidity

        //visibility
        this.visibility=res.data.current.visibility

        //uvi
        this.uvi=res.data.current.uvi

        //windspeed
        this.windspeed=res.data.current.wind_speed

        //GET request for only finding city(current location)
         axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${this.latitude}&lon=${this.longitude}&appid=6fdeade888587f628e2b2739b5d83466&units=metric`).then((res)=>{
         //console.log(res.data)
         //current-location-name(curLocationName)
         this.curLocationName=res.data.name

         //highTemp
         this.highTemp=res.data.main.temp_max

         //lowTemp
         this.lowTemp=res.data.main.temp_min/1000
        
         })
        // let unix=(res.data.sys.sunrise)
        // let dat=new Date(unix*1000)
        // let unix1=res.data.sys.sunset
        // let dat1=new Date(unix1*1000)
        
      })
    }else{

         //GET request for weather and all
         axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.recentLatitude}&lon=${this.recentLongitude}&appid=6fdeade888587f628e2b2739b5d83466&units=metric`).then((res)=>{
         console.log(res.data);

         //currenticon(mainicon)
         let cur_icon=res.data.current.weather[0].icon
         this.currenticon=`https://openweathermap.org/img/w/${cur_icon}.png`

         //timezone
         this.timezone=res.data.timezone

         //for getting current time
         let unix=(res.data.current.dt)
         let dat=new Date(unix*1000)
         this.currentTime=dat

         //temperature
         this.temperature=Math.ceil(res.data.current.temp)

         //weather
         this.weather=res.data.current.weather[0].main

         //feelslike
         this.feelsLike=Math.ceil(res.data.current.feels_like)

         //sunrise
         let unix1=(res.data.current.sunrise)
         let dat1=new Date(unix1*1000)
         this.sunrise=dat1.toLocaleTimeString();
        
         //sunset
         let unix2=(res.data.current.sunset)
         let dat2=new Date(unix2*1000)
         this.sunset=dat2.toLocaleTimeString();


     
        //hourly-Data
         let hour1=(res.data.hourly[0].dt)
         let hour11=new Date(hour1*1000)
         this.hourlyArr=hour11.toLocaleTimeString()

         let hour2=(res.data.hourly[1].dt)
         let hour22=new Date(hour2*1000)
         this.hourlyArr1=hour22.toLocaleTimeString()

         let hour3=(res.data.hourly[2].dt)
         let hour33=new Date(hour3*1000)
         this.hourlyArr2=hour33.toLocaleTimeString()

         let hour4=(res.data.hourly[3].dt)
         let hour44=new Date(hour4*1000)
         this.hourlyArr3=hour44.toLocaleTimeString()

         
         //hourly-weather
         this.hourlyWeather1=res.data.hourly[0].weather[0].main
         this.hourlyWeather2=res.data.hourly[1].weather[0].main
         this.hourlyWeather3=res.data.hourly[2].weather[0].main
         this.hourlyWeather4=res.data.hourly[3].weather[0].main


        //hourly-weather images
         let icon1=res.data.hourly[0].weather[0].icon
         this.hourlyicon1=`https://openweathermap.org/img/w/${icon1}.png`;

         let icon2=res.data.hourly[1].weather[0].icon
         this.hourlyicon2=`https://openweathermap.org/img/w/${icon2}.png`;

         let icon3=res.data.hourly[2].weather[0].icon
         this.hourlyicon3=`https://openweathermap.org/img/w/${icon3}.png`;

         let icon4=res.data.hourly[3].weather[0].icon
         this.hourlyicon4=`https://openweathermap.org/img/w/${icon4}.png`;




         //Daily-data
         let daily1=(res.data.daily[0].dt)
         let daily11=new Date(daily1*1000)
         this.dailyArr=daily11.toLocaleDateString()

         let daily2=(res.data.daily[1].dt)
         let daily22=new Date(daily2*1000)
         this.dailyArr1=daily22.toLocaleDateString()

         let daily3=(res.data.daily[2].dt)
         let daily33=new Date(daily3*1000)
         this.dailyArr2=daily33.toLocaleDateString()

         let daily4=(res.data.daily[3].dt)
         let daily44=new Date(daily4*1000)
         this.dailyArr3=daily44.toLocaleDateString()


        //daily-weather
         this.dailyWeather1=res.data.daily[0].weather[0].main
         this.dailyWeather2=res.data.daily[2].weather[0].main
         this.dailyWeather3=res.data.daily[2].weather[0].main
         this.dailyWeather4=res.data.daily[2].weather[0].main


        //daily-weather images
         let dicon1=res.data.daily[0].weather[0].icon
         this.dailyicon1=`https://openweathermap.org/img/w/${dicon1}.png`;

         let dicon2=res.data.daily[1].weather[0].icon
         this.dailyicon2=`https://openweathermap.org/img/w/${dicon2}.png`;

         let dicon3=res.data.daily[2].weather[0].icon
         this.dailyicon3=`https://openweathermap.org/img/w/${dicon3}.png`;

         let dicon4=res.data.daily[3].weather[0].icon
         this.dailyicon4=`https://openweathermap.org/img/w/${dicon4}.png`;

        //humdity
        this.humidity=res.data.current.humidity

         //visibility
        this.visibility=res.data.current.visibility/1000

         //uvi
        this.uvi=res.data.current.uvi

        //windspeed
        this.windspeed=res.data.current.wind_speed


        //GET request for only finding city(current location)
         axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${this.recentLatitude}&lon=${this.recentLongitude}&appid=6fdeade888587f628e2b2739b5d83466&units=metric`).then((res)=>{
         //console.log(res.data)
         //current-location-name(curLocationName)
         this.curLocationName=res.data.name

         //highTemp
         this.highTemp=res.data.main.temp_max

         //lowTemp
         this.lowTemp=res.data.main.temp_min
        
         })
        // let unix=(res.data.sys.sunrise)
        // let dat=new Date(unix*1000)
        // let unix1=res.data.sys.sunset
        // let dat1=new Date(unix1*1000)
        
      })
    this.compmount=true

    }
        },
        userSearch(){
           
        //for get latitude and longitude from user
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.inpvalue}&appid=6fdeade888587f628e2b2739b5d83466&units=metric`).then((res)=>{
        //console.log(this.inpvalue)
        this.latitude=res.data.coord.lat
        this.longitude=res.data.coord.lon

        //setting local stroe age
        localStorage.setItem("latitude",JSON.stringify(this.latitude))
        localStorage.setItem("longitude",JSON.stringify(this.longitude))

        //current-location-name(curLocationName)
        this.curLocationName=res.data.name

         //highTemp
         this.highTemp=res.data.main.temp_max

         //lowTemp
         this.lowTemp=res.data.main.temp_min
         this.userSearch2()
            })
         this.inpvalue=''
        },
        userSearch2(){
            console.log("search2")
            //finding weather using lat and lon
        axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.latitude}&lon=${this.longitude}&appid=6fdeade888587f628e2b2739b5d83466&units=metric`).then((res)=>{
        console.log(this.inpvalue)

        //currenticon(mainicon)
         let cur_icon=res.data.current.weather[0].icon
         this.currenticon=`https://openweathermap.org/img/w/${cur_icon}.png`

        //timezone
         this.timezone=res.data.timezone

         //for getting current time
         let unix=(res.data.current.dt)
         let dat=new Date(unix*1000)
         this.currentTime=dat

         //temperature
         this.temperature=Math.ceil(res.data.current.temp)

         //weather
         this.weather=res.data.current.weather[0].main

         //feelslike
         this.feelsLike=Math.ceil(res.data.current.feels_like)

         //sunrise
         let unix1=(res.data.current.sunrise)
         let dat1=new Date(unix1*1000)
         this.sunrise=dat1.toLocaleTimeString();
        
         //sunset
         let unix2=(res.data.current.sunset)
         let dat2=new Date(unix2*1000)
         this.sunset=dat2.toLocaleTimeString();

         //hourly-Data
         let hour1=(res.data.hourly[0].dt)
         let hour11=new Date(hour1*1000)
         this.hourlyArr=hour11.toLocaleTimeString()

         let hour2=(res.data.hourly[1].dt)
         let hour22=new Date(hour2*1000)
         this.hourlyArr1=hour22.toLocaleTimeString()

         let hour3=(res.data.hourly[2].dt)
         let hour33=new Date(hour3*1000)
         this.hourlyArr2=hour33.toLocaleTimeString()

         let hour4=(res.data.hourly[3].dt)
         let hour44=new Date(hour4*1000)
         this.hourlyArr3=hour44.toLocaleTimeString()

         
         //hourly-weather
         this.hourlyWeather1=res.data.hourly[0].weather[0].main
         this.hourlyWeather2=res.data.hourly[1].weather[0].main
         this.hourlyWeather3=res.data.hourly[2].weather[0].main
         this.hourlyWeather4=res.data.hourly[3].weather[0].main


        //hourly-weather images
         let icon1=res.data.hourly[0].weather[0].icon
         this.hourlyicon1=`https://openweathermap.org/img/w/${icon1}.png`;

         let icon2=res.data.hourly[1].weather[0].icon
         this.hourlyicon2=`https://openweathermap.org/img/w/${icon2}.png`;

         let icon3=res.data.hourly[2].weather[0].icon
         this.hourlyicon3=`https://openweathermap.org/img/w/${icon3}.png`;

         let icon4=res.data.hourly[3].weather[0].icon
         this.hourlyicon4=`https://openweathermap.org/img/w/${icon4}.png`;




         //Daily-data
         let daily1=(res.data.daily[0].dt)
         let daily11=new Date(daily1*1000)
         this.dailyArr=daily11.toLocaleDateString()

         let daily2=(res.data.daily[1].dt)
         let daily22=new Date(daily2*1000)
         this.dailyArr1=daily22.toLocaleDateString()

         let daily3=(res.data.daily[2].dt)
         let daily33=new Date(daily3*1000)
         this.dailyArr2=daily33.toLocaleDateString()

         let daily4=(res.data.daily[3].dt)
         let daily44=new Date(daily4*1000)
         this.dailyArr3=daily44.toLocaleDateString()


        //daily-weather
         this.dailyWeather1=res.data.daily[0].weather[0].main
         this.dailyWeather2=res.data.daily[2].weather[0].main
         this.dailyWeather3=res.data.daily[2].weather[0].main
         this.dailyWeather4=res.data.daily[2].weather[0].main


        //daily-weather images
         let dicon1=res.data.daily[0].weather[0].icon
         this.dailyicon1=`https://openweathermap.org/img/w/${dicon1}.png`;

         let dicon2=res.data.daily[1].weather[0].icon
         this.dailyicon2=`https://openweathermap.org/img/w/${dicon2}.png`;

         let dicon3=res.data.daily[2].weather[0].icon
         this.dailyicon3=`https://openweathermap.org/img/w/${dicon3}.png`;

         let dicon4=res.data.daily[3].weather[0].icon
         this.dailyicon4=`https://openweathermap.org/img/w/${dicon4}.png`;


        //humdity
        this.humidity=res.data.current.humidity

         //visibility
        this.visibility=res.data.current.visibility/1000

         //uvi
        this.uvi=res.data.current.uvi

        //windspeed
        this.windspeed=res.data.current.wind_speed
     
        })
        
        }
    },
    created(){
        this.trackMyLocation()
    },
   

}
</script>


<style scoped>
input{
    margin-bottom: 5%;
}
.lcontainer{
    padding: 10px;
    border:1px solid wheat;
    height: 62vh;
    width: 40%;
    display: inline-block;
    margin:5%;
    border-radius: 5%;
    background-color: white;
    text-align: left;
    /* color: white;
    background-image: url(https://cdn.pixabay.com/photo/2018/01/14/23/12/nature-3082832__340.jpg); */
    box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
}
.h1{
    color:white;
}
.loc{
    display: inline-block;
    margin-top: 0%;
   
}
.lcontainer input{
    margin-top: 5%;
    border:1px solid orange;
    width: 70%;
    height: 8%;
    border-radius: 5px;
}
.lcontainer button{
    margin-left: 2%;
    height: 8%;
    border:1px solid orange;
    border-radius: 5px;
    cursor: pointer;
}
.lcontainer input:focus{
        outline: none;
}
.curr-time{
  margin-top: -2%;
}
.temp{
    color:orange;
    font-size: 50px;
    margin-top: 2%;
}
.weather{
     margin-top: -6%;
}
.sun{
    display: inline-block;
    margin: 5px;
}
.weathericon{
    font-size: 50px;
    /* margin-top: 4%; */
}
.btn{
  background-color: orange;
  color: white;
}
.curr-icon{
    height: 10vh;
    margin-top: -2%;
}
@media (max-width:500px) {
.lcontainer{
    display: block;
    width: 99%;
    margin:0;
    height:65vh;
    padding:2px;
    border-radius: 4px;
    margin-left: 0;
    font-size: 14px;
}
}
@media (min-width:501px) and (max-width:1200px) {
    .lcontainer{
        height: 65vh;
        width: 50%;
    }
}


</style>
