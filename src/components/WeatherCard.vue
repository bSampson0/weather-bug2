<template>
  <div class="weather-card">
    <v-container fluid>
      <v-row dense>
        <v-col>
          <v-card
            class="mx-auto"
            max-width="400"
            max-height="700"
            dark
          >
            <v-img
              class="white--text"
              gradient="to bottom, rgba(0,0,0,.5), rgba(0,0,0,.1)"
              src="https://images.unsplash.com/photo-1530986272794-dff9b6f11d7c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60"
            >
              <div class="today-report">
                <div class="date">
                  <p class="text-center">{{today}}</p>
                </div>
                <div class="weather-icon text-center">
                  <v-row>
                    <v-col cols="12">
                      <h1 class="bold pb-8 text-center">{{tempF}}&deg;F</h1>
                    </v-col>
                    <v-col cols="12" align="center" class="nudge-up">
                      <v-img 
                        :src="imgSrc"
                        max-height="70px" 
                        max-width="70px" 
                        >
                      </v-img>
                    </v-col>
                  </v-row>
                  <!-- <i class="mdi mdi-white-balance-sunny"></i> -->
                </div>
                <!-- <div class="current-temp text-center">
                  <p style="margin-bottom: -.5em">Feels Like</p>
                  <h1 class="bold">{{feelsLike}}&deg;F</h1>
                </div> -->
                <v-card-text>
                  <div class="forecast text-center">
                    <p class="temp">High: {{maxTempF}}&deg;F | Low: {{minTempF}}&deg;F</p>
                  </div>
                </v-card-text>
                <div class="location">
                  <p>{{city}}</p>
                </div>
              </div>
            </v-img>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
  export default {
    props: ['city', 'feelsLike', 'tempF', 'imgUrl', 'imgAlt', 'windSpeed', 'humidity', 'maxTempF', 'minTempF'],
    data () {
      return {
        options: { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' },
        today: new Date(),
      }
    },
    mounted() {
        this.today = this.today.toLocaleString('en-US', this.options)
    },
    computed: {
      imgSrc() {
        if (!this.imgUrl) {
         return ''
      } else {
        return this.imgUrl
      }
    },
    imgText() {
      if (!this.imgAlt) {
        return 'image not found'
      } else {
        return this.imgAlt
      }
    },
    }
  }
</script>

<style>

  .nudge-up {
    margin-top: -4em;
  }
  .location {
    position: absolute;
    bottom: 0px;
    left: 50%;
    transform: translate(-50%);
  }

  .mdi {
    font-size: 4em;
  }

  .weather-icon {
    margin-top: 1em;
  }

  .date {
    margin-top: 1em;
  }

  .date p {
    font-size: 1.2em;
  }

  .bold {
    font-weight: 900;
  }

  .v-text-field__slot {
    color: #fff !important;
  }

  .today-report {
    padding-bottom: 4em;
  }



</style>