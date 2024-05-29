<template>
 <div class="container p-0">
    <div class="d-flex mt-4">
        <div class="card main-div w-100">
            <div class="p-3">
                <h2 class="mb-1 day">Heute</h2>
                <p class="text-light date mb-0">{{date}}</p>
                <small>{{time}}</small>
                <h2 class="place"><i class="fa fa-location">{{name}} <small>{{country}}</small></i></h2>
                <div class="temp">
                    <h1 class="weather-temp">{{temperature}}&deg;</h1>
                    <h2 class="text-light">{{description}} <img :src="iconUrl"></h2>
                </div>
            </div>
        </div>
        <div class="card card-2 w-100">
        <table class="m-4">
            <tbody>
                <tr>
                    <th>Meeresspiegel</th>
                    <td v-if="sea_level">{{ sea_level }}</td>
                    <td v-else > <h2>&#128511;</h2></td>
                </tr>
                <tr>
                    <th>Luftfeuchtigkeit</th>
                    <td>{{ humidity }}</td>
                </tr>
                <tr>
                    <th>Wind</th>
                    <td>{{ wind }}</td>
                </tr>
            </tbody>
        </table>

        <DaysWeather :cityname="cityname"></DaysWeather>
        <div id="div_Form" class="d-flex m-3 justify-content-center">
            <form action="">
                <input type="button" value="change Location" @click="changeLocation" class="btn change_btn btn-primary">
            </form>
        </div>
    </div>
    </div>


 </div>
 

    </template>
    
    <script>
    import axios from "axios";
     import DaysWeather from './DaysWeather.vue'
    export default (await import('vue')).defineComponent({
      name: 'myWeather',
      components: {
        DaysWeather,
      },
      props: {
        city: String,
      },
      data(){
        return {
            cityname: this.city,
            temperature: null,
            description: null,
            iconUrl: null,
            data: null,
            time: null,
            name: null,
            sea_level: null,
            wind: null,
            country: null,
            humidity: null,

            monthNames: ["Januar", "Februar", "März", "April", "Mai", "Juni", "Juli", "August", "September", "Oktober", "November", "Dezember"],

        }
      },
      methods: {
        changeLocation(){
            window.location.reload();
        }
      },
      async created(){
        const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8331cdef4f10633c84fd856ce65588b0`);
        const weatherData =  response.data;
        this.temperature =  weatherData.main.temp;
        this.discription =  weatherData.weather[0].description;
        this.name =  weatherData.name;
        this.wind =  weatherData.wind.speed;
        this.sea_level =  weatherData.main.sea_level;
        this.country =  weatherData.sys.country;
        this.humidity =  weatherData.main.humidity;
        this.iconUrl =  `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
        const d = new Date();
        this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
        this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();

        console.log(response);
      }
    })
    
    </script>
    
    
    <style>

    body{
        background-color: #343d4b !important;

    }
    .card {
        background-color: #343d4b !important;
    }
    .card-2 {
        background-color: #343d4b !important;
    }
    .weather-temp{
        color: #fff;
        margin: 0;
        font-weight: 700;
        font-size: 4rem;
    }
    .p-3 {

  color: #fff;
}

    h2.mb-1.day{
        font-size: 3rem;
        font-weight: 400;

    }
    .main-div{
        border-radius: 20px;
        color: #fff;
        background-image: url('/public/img/clouds-3488632_1280.jpg');
        background-size: cover;
        background-position: center;
        background-blend-mode: overlay;
        background-color: rgba(0, 0, 0, 0.5);
        background-repeat: no-repeat;

    }
    .temp{
        position: absolute;
        bottom: 0;
    }
    .main-div:hover {
        transform: scale(1.1);
        transition: transform 0.5s ease;
        z-index: 1;
    }
    .card-2 {
        background-color: #212730;
        border-radius: 20px;
    }

    .card-details {
        margin-left: 19px;
    }
    .h1_left{
        position: absolute;
        bottom: 25px;
        left: 16px;
        font-size: 3vw;
        line-height: 1.2;
    }
    .h3_left {
        position: absolute;
        left: 16px; 
        font-size: 2vw;
        line-height: 0.5;
    }

    .h3_left small {
        font-size: 1rem;
    }

    table{
        position: relative;
        left: 15px;
        border-collapse: separate;
        border-spacing: 15px;
        width: 85%;
        text-align: left;
        max-width: 600px;
        margin: 0 auto;
    }

    th,td {
        font-size: 18px;
        color: #fff;
    }

    td {
        text-align: right;
    }


    table, tr:hover{
        color: red;
    }

    .change_btn{
        background-image: linear-gradient(to right, purple, purple);
    }

    .change_btn:hover{
        transform: scale(0.9);
        transition: transform 0.1s ease;
    }
    
    </style>