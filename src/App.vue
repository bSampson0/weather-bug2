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
              :city = "data.name"
              :tempF = "parseInt(data.main.feels_like)"
              :imgUrl = "imgUrl"
              :imgAlt = "data.weather[0].description"
              :windSpeed= "data.wind.speed"
              :humidity= "data.main.humidity"
              :maxTempF = "parseInt(data.main.temp_max)"
              :minTempF = "parseInt(data.main.temp_min)"
            />
          </div>
          <div v-else class="waiting text-center mt-10">
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
    data: {},
    isLoaded: false,
    zipCode: '97701',
    imgUrl: '',
    key: process.env.VUE_APP_API_KEY
  }),
  created() {
      this.getWeather()
    },
    methods: {
      async getWeather () {
        this.isLoaded = false
        this.menuOpen = false
        try {
          await axios({
            url: `https://community-open-weather-map.p.rapidapi.com/find`,
            method: 'GET',
            headers: {"X-RapidAPI-Key": this.key},
            params: {
              "type": "link%2C accurate",
              "units": "imperial",
              "q": this.zipCode + ',us'
            }
          }).then(resp => {
            return resp.data.list[0]
          }).then(data => {
            this.data = data
            this.imgUrl = `http://openweathermap.org/img/wn/${this.data.weather[0].icon}@2x.png`
            this.isLoaded = true
          })
          } catch (error) {
            console.log(error)
          }
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
