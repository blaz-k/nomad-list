<template>
  <!-- <div v-if="weatherData && covidResults">
    <p>
      Temperature in {{ showCountryCapital.country }},
      {{ showCountryCapital.city }} is: {{ weatherData.main.temp }}°C, results:
      {{ covidResults.data }}
    </p>
  </div> -->

  <div id="about" v-if="weatherData">
    <div
      class="card mt-4"
      @click="cardOne == 'start' ? (cardOne = 'flipped') : (cardOne = 'start')"
      v-bind:class="{ flipme: cardOne == 'flipped' }"
    >
      <section class="card-list card__face card__face--front">
        <article class="card">
          <header class="card-header">
            <p>{{ newDate }}</p>
            <h2>{{ showCountryCapital.country }}</h2>
          </header>
          <div class="card-author">
            <a href="#" class="author-avatar">
              <img src="../assets/logo.png" alt=""
            /></a>
            <svg class="half-circle" viewBox="0 0 106 57">
              <path d="M102 4c0 27.1-21.9 49-49 49S4 31.1 4 4"></path>
            </svg>
            <div class="author-name">
              <div class="author-name-prefix">Author</div>
              Blaz Kmetic
            </div>
          </div>
        </article>
      </section>

      <section class="card-list card__face card__face--back">
        <div>
          <article class="card">
            <header class="card-header">
              <p>{{ newDate }}</p>
              <h2>{{ showCountryCapital.country }}</h2>

              <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Country capital is: </span>
                <span class="badge-right">{{
                  showCountryCapital.city
                }}</span></span
              >
              <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Population: </span>
                <span class="badge-right">{{
                  covidResults.response[0].population
                }}</span></span
              >

              <!-- <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Confirmed Covid19 cases:</span>
                <span class="badge-right">
                  {{ covidResults[countryCovid].confirmed }}</span
                ></span
              > -->

              <span class="badge rounded-pill bg-none mt-3">
                <span class="badge-right">COVID-19 SITUATION:</span></span
              >

              <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Active cases: </span>
                <span class="badge-right">{{
                  covidResults.response[0].cases.active
                }}</span></span
              >

              <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Critical: </span>
                <span class="badge-right">{{
                  covidResults.response[0].deaths.total
                }}</span></span
              >

              <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Total deaths: </span>
                <span class="badge-right">{{
                  covidResults.response[0].deaths.total
                }}</span></span
              >
            </header>
            <!-- <div class="card-author">
              <div class="author-name">
                <div class="author-name-prefix">Author</div>
                Blaz Kmetic
              </div>
            </div> -->
          </article>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import axios from "axios";
import moment from "moment";

export default {
  name: "ShowCountries",
  props: ["showCountryCapital"],
  computed: {
    ...mapState(["countriesAndCapitals"]),
    newDate() {
      let date = new Date(this.covidResults.response[0].time);
      return moment(date).format("MMM Do YYYY");
    },
  },
  created() {
    this.getWeatherData();
    this.getCovid();
    // this.getLandmarksAndHotels();
    // this.getPopulationArea();
    // this.getCovid();
    // this.getBestRatedHotel();
  },

  data() {
    return {
      cardOne: "start",
      sameApi: "c90dc18f0bmsh5e0e2cf7a230c4ep1b4723jsn4bbafe8dc12a",

      //Weather data:
      weatherApi: "a04401ce5a86509d82dbf7cafc6d2e6f",
      weatherUrl: "https://api.openweathermap.org/data/2.5/weather?q=",
      weatherData: null,
      units: "metric",
      //Covid data:
      covidResults: null,
      countryCovid: this.showCountryCapital.country,

      //Population data:
      populationURl: "https://spott.p.rapidapi.com/places/",
      countryId: this.showCountryCapital.abbrev,
      populationArea: null,

      //Landmark and hotels data:
      landmarksHotels: null,

      // Best Hotel data:
      bestHotel: null,
    };
  },
  methods: {
    //SHOW BEST RATED HOTEL IN CITY:
    getBestRatedHotel() {
      const options = {
        method: "GET",
        url: "https://best-booking-com-hotel.p.rapidapi.com/booking/best-accommodation",
        params: {
          cityName: this.showCountryCapital.city,
          countryName: this.showCountryCapital.country,
        },
        headers: {
          "x-rapidapi-host": "best-booking-com-hotel.p.rapidapi.com",
          "x-rapidapi-key":
            "c90dc18f0bmsh5e0e2cf7a230c4ep1b4723jsn4bbafe8dc12a",
        },
      };

      axios
        .request(options)
        .then((response) => {
          this.bestHotel = response.data;
          console.log(this.bestHotel);
        })
        .catch(function (error) {
          console.error(error);
        });
    },

    // GET LANDMARK || HOTEL REQUEST : (shows hotels, from which transport to hotel in what time landmarks name, transports(train-station, airport, station) for certain ciy  )
    //  - to do: show all landmarks name for chosen city
    // const options = {
    //   method: 'GET',
    //   url: 'https://hotels4.p.rapidapi.com/properties/get-details',
    //   params: {
    //     id: '1078494784',
    //     checkIn: '2020-01-08',
    //     checkOut: '2020-01-15',
    //     adults1: '1',
    //     currency: 'USD',
    //     locale: 'en_US'
    //   },
    //   headers: {
    //     'x-rapidapi-host': 'hotels4.p.rapidapi.com',
    //     'x-rapidapi-key': 'c90dc18f0bmsh5e0e2cf7a230c4ep1b4723jsn4bbafe8dc12a'
    //   }
    // };

    // axios.request(options).then(function (response) {
    // 	console.log(response.data);
    // }).catch(function (error) {
    // 	console.error(error);
    // });

    //from const options=> here; all hotels data

    // import axios from "axios";

    // const options = {
    //   method: 'GET',
    //   url: 'https://hotels4.p.rapidapi.com/properties/get-hotel-photos',
    //   params: {id: '1078494784'},
    //   headers: {
    //     'x-rapidapi-host': 'hotels4.p.rapidapi.com',
    //     'x-rapidapi-key': 'c90dc18f0bmsh5e0e2cf7a230c4ep1b4723jsn4bbafe8dc12a'
    //   }
    // };

    // axios.request(options).then(function (response) {
    // 	console.log(response.data);
    // }).catch(function (error) {
    // 	console.error(error);
    // });

    //do tukaj za slike

    getLandmarksAndHotels() {
      const options = {
        method: "GET",
        url: "https://hotels4.p.rapidapi.com/locations/v2/search",
        params: {
          query: this.showCountryCapital.city,
          locale: "EUR",
          currency: "EUR",
        },
        headers: {
          "x-rapidapi-host": "hotels4.p.rapidapi.com",
          "x-rapidapi-key": this.sameApi,
        },
      };

      axios
        .request(options)
        .then((response) => {
          this.landmarksHotels = response;
          console.log("GET landmark REQUEST");
          console.log(this.landmarksHotels);
        })
        .catch(function (error) {
          console.error(error);
        });
    },
    // GET COVID REQUEST : (shows confirmed, deaths, active, critical, day, new, for 1M population || tests total, for 1M pop,  for certain country)
    getCovid() {
      const options = {
        method: "GET",
        url: "https://covid-193.p.rapidapi.com/statistics",
        params: { country: this.countryCovid },
        headers: {
          "x-rapidapi-host": "covid-193.p.rapidapi.com",
          "x-rapidapi-key": this.sameApi,
        },
      };

      axios
        .request(options)
        .then((response) => {
          this.covidResults = response.data;
          console.log(this.covidResults);
        })
        .catch(function (error) {
          console.error(error);
        });
    },
    // GET POPULATION AND AREA REQUEST( shows area, population)
    getPopulationArea() {
      const options = {
        method: "GET",
        url: `https://spott.p.rapidapi.com/places/${this.countryId}`,
        headers: {
          "x-rapidapi-host": "spott.p.rapidapi.com",
          "x-rapidapi-key": this.sameApi,
        },
      };

      axios
        .request(options)
        .then((response) => {
          this.populationArea = response.data;
          // console.log(this.populationArea);
        })
        .catch(function (error) {
          console.error(error);
        });
    },

    // GET WEATHER REQUEST : (shows all weather data for certain city)
    async getWeatherData() {
      try {
        let res = await axios.get(
          `${this.weatherUrl}${this.showCountryCapital.city}&units=${this.units}&appid=${this.weatherApi}`
        );
        this.weatherData = res.data;
        console.log(this.weatherData);
      } catch (error) {
        let res = await axios.get(
          `${this.weatherUrl}${this.showCountryCapital.country}&units=${this.units}&appid=${this.weatherApi}`
        );
        this.weatherData = res.data;
        // console.log(this.weatherData);
      }
    },
  },
};
</script>

<style></style>

@media screen and (min-device-width: 1024px) and (max-device-width: 1600px) and
(-webkit-min-device-pixel-ratio: 2) and (min-resolution: 192dpi) { body { } }
@media only screen and (min-device-width: 600px) and (max-device-width: 1024px)
and (-webkit-min-device-pixel-ratio: 1) { body { } #about .card { display: flex;
position: relative; flex-direction: column; height: 350px; width: 400px;
min-width: 150px; padding: 0.9rem; border-radius: 26px; background: #1d1413;
box-shadow: -1rem 0 4em rgb(184, 151, 2); /* margin-left: 50px; */ transition:
0.2s; } } @media only screen and (min-device-width: 320px) and
(max-device-width: 600px) and (-webkit-min-device-pixel-ratio: 2) { body {
background-color: yellow; } #about .card { display: flex; position: relative;
flex-direction: column; height: 350px; width: 400px; min-width: 150px; padding:
0.9rem; border-radius: 26px; background: #1d1413; box-shadow: -1rem 0 4em
rgb(163, 2, 184); /* margin-left: 50px; */ transition: 0.2s; } }
