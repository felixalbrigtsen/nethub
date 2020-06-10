<template>
  <div class="weather">
        <label class="header">Forecast for <input v-bind:value="value" @input="$emit('input', $event.target.value)" @change="getWeather()" /></label>
        <div v-if="error" class="error">{{errorMsg}}</div>
        <div v-if="dailyWeather.length==0">Loading...</div>
        <div class="days">
            <div v-for="day in dailyWeather.filter(day=>day.index==1 || width > 1)" :key="day.index" class="weatherCard">
                <h3>{{day.description}}</h3>
                <div><img :src="day.imgurl" /></div>
                <h4>{{(day.temperature - 273.15).toFixed(1).toString() + "°"}}
                <br>{{["Today","Tomorrow","In two days"][day.index]}}</h4>
            </div>
        </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
    name: "ModuleWeather",
    props: ["value", "width"],
    data() {
        return {
            error: false,
            errorMsg: "",
            dailyWeather: []
        }
    },
    methods: {
        capitalizeFirst(text) {
            let words = text.split(" ").map(word=>(word[0].toUpperCase()+word.slice(1)))
            return words.join(" ")
        },
        parseWeather(days) {
            this.dailyWeather = []
            days.forEach((day,idx) => {
                this.dailyWeather.push({
                    index: idx,
                    description: this.capitalizeFirst(day.weather[0].description),
                    weather: day.weather[0].main,
                    imgurl: "http://openweathermap.org/img/wn/"+day.weather[0].icon+"@2x.png",
                    temperature: day.main.temp,
                    wind: day.wind
                })
            })
            this.error = false
        },
        getWeather() {
            //Get 3 day weather forecast
            if (!this.value) {
                this.$emit("input","Oslo,NO")
            }
            const url = "http://api.openweathermap.org/data/2.5/forecast?q="+encodeURI(this.value)+"&cnt=3&APPID=d4a047f3be1fa21d6f4016c1bb81bfbc"
            axios.get(url)
                .then(response=>{this.parseWeather(response.data.list)})
                .catch((err)=>{this.errorMsg = err; this.error = true})
        }
    },
    mounted() {
        if (!this.value) {
            this.$emit("input","Oslo,NO")
        }
        setTimeout(this.getWeather,100)
    }
}
</script>

<style scoped>
.weather {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    font-family: "Lucida Console", Monaco, monospace;
    text-align: center;
    padding: 0 5px;
}
.header {
    text-align: center;
    font-size: 16px;
    margin: 6px;
    width: 100%;
}
.header > input {
    width: 35%;
    background-color: lightgray;
    border: 1px solid gray;
    border-radius: 3px;
    font-size: 14px;
    text-align: center;
}
.days {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin: 5px;
    margin-bottom: 16px;
    width: 100%;
    height: 80%;
    text-align: center;
}
.weatherCard {
    margin: 0 5px;
    width: 100%;
    height: 100%;
    display: block;
    border: 1px solid gray;
    border-radius: 10px;
    background-color: lightslategray;
    color: white;
    text-align: center;
    display: flex;
    flex-direction: column;
}
.weatherCard > h3 {
    height: 35px;
}
img {
    background-color: skyblue;
    border: 1px solid black;
    border-radius: 20px;
}

.error {
    font-size: 24px;
    color: red;
    text-align: center;
}
</style>
<style>
.grid-1-1>.moduleContainer>.weather>.days>.weatherCard>h3{
    height: unset;
}
</style>