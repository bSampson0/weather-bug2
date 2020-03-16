<template>
  <v-app>
    <v-content>
      <v-container>
        <v-row align="center" justify="center">
          <v-col
           cols="12"
           xs="12"
           sm="8"
           md="6"
          >
            <v-card
        class="mx-auto"
        max-width="400"
        color="#333" 
        d-flex           
      >
        <v-toolbar
            color="blue-grey"
            dark
          >
            <v-toolbar-title><v-icon>mdi-weather-lightning</v-icon> <span class="bold">Weather</span>Wiz</v-toolbar-title>

            <v-spacer></v-spacer>
            
            <v-menu offset-y left :close-on-content-click="false" v-model="menuOpen">
              <template v-slot:activator="{ on }">
                <v-btn icon v-on="on">
                  <v-icon>mdi-magnify</v-icon>
                </v-btn>
              </template>
              <v-card 
                class="pa-4"
                color="blue-grey darken-3"
                dark
              >
                <v-card-content>
                    <v-text-field
                        v-on:keyup.enter="getWeather"
                        v-model="zipCode"
                        label="Zip Code"
                    ></v-text-field>
                    <v-card-actions justify="center">
                      <v-btn @click="getWeather">
                        Get Weather
                      </v-btn>
                    </v-card-actions>
                </v-card-content>
            </v-card>
            </v-menu>
            
          </v-toolbar>
          <div v-if="isLoaded" class="weather-widget">
            <WeatherCard
              :city = "city"
              :state = "state"
              :localtime = "localtime"
              :tempF = "tempF"
              :imgUrl = "imgUrl"
              :windSpeed= "windSpeed"
              :humidity= "humidity"
              :fullReportUrl= "fullReportUrl"
              :tomTempF = "tomTemp"
              :maxTempF = "maxTempF"
              :minTempF = "minTempF"
            />
          </div>
          <div v-else class="waiting text-center">
            <v-progress-circular
              indeterminate
              color="primary"
            ></v-progress-circular>
          </div>
          <v-card-actions>
            <div class="footer text-center">
              <span>Created by: Bnice | &copy; 2019</span>
            </div>
          </v-card-actions>
          </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import WeatherCard from '@/components/WeatherCard.vue';
import axios from 'axios'

export default {
  name: "App",
  components: {
    WeatherCard
  },
  data: () => ({
    menuOpen: false,
    tempF: null,
    lastUpdated: null,
    currentCondition: null,
    tomTemp: null,
    tomCondition:null,
    zipCode: '97701',
    city: null,
    state: null,
    localtime: null,
    imgUrl: null,
    imgAlt: null,
    windSpeed: null,
    humidity: null,
    fullReportUrl: null,
    minTempF: null,
    maxTempF:null,
    lat: null,
    lon: null,
    isLoaded: false,
    key: 'd1f37ccd92a94a5ba8a21313202701'
  }),
  created() {
      this.getWeather()
    },
    methods: {
      getWeather () {
        this.isLoaded = false
        axios.get(`http://api.weatherapi.com/v1/forecast.json?key=${this.key}&q=${this.zipCode}&days=1`)
          .then(resp => {
            return resp.data
          }).then(data => {
            this.tempF = parseInt(data.current.temp_f)
            this.lastUpdated = data.current.last_updated
            this.currentCondition = data.current.condition.text
            this.maxTempF = parseInt(data.forecast.forecastday[0].day.maxtemp_f)
            this.minTempF = parseInt(data.forecast.forecastday[0].day.mintemp_f)
            this.city = data.location.name
            this.state = data.location.region
            this.localtime = data.location.localtime
            this.imgUrl = 'http:' + data.current.condition.icon
            this.imgAlt = data.current.condition.text
            this.windSpeed = data.current.wind_mph
            this.humidity = data.current.humidity
            this.lat = data.location.lat
            this.lon = data.location.lon
            this.fullReportUrl = `https://forecast.weather.gov/MapClick.php?lat=${this.lat}&lon=${this.lon}`
            console.log(this.imgAlt)
            console.log(data)
          })
          this.menuOpen = false
          this.isLoaded = true
      },
      
    }
}
</script>

<style>
  .zipcode {
    margin: auto;
  }
  .v-content {
    font-family: 'Raleway', sans-serif;
  }
  .waiting {
    height: 500px;
    width: 400px;
  }
  .v-card__actions {
    background: #607D8B;
  }
  .footer {
    color: #fff;
  }
  .name {
    float: right;
  }
</style>
