<template>
  <!-- <div v-if="weatherData && covidResults">
    <p>
      Temperature in {{ showCountryCapital.country }},
      {{ showCountryCapital.city }} is: {{ weatherData.main.temp }}°C, results:
      {{ covidResults.data }}
    </p>
  </div> -->

  <div id="show">
    <div v-if="populationArea">
      Populationarea e obstaja:
      {{ populationArea }}
    </div>

    <section class="card-list" v-if="weatherData">
      <article class="card">
        <header class="card-header">
          <p>May 25th 2020</p>
          <h2>
            Temperature in {{ showCountryCapital.city }},
            {{ showCountryCapital.country }} is:
          </h2>
          <h3>{{ Math.round(weatherData.main.temp) }}°C</h3>
          <p>
            <!-- There has been {{ covidResults[countryCovid].confirmed }} confirmed
            casses since the start of covid. -->
          </p>
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
  </div>
</template>

<script>
import { mapState } from "vuex";
import axios from "axios";

export default {
  name: "ShowCountries",
  props: ["showCountryCapital"],
  computed: {
    ...mapState(["countriesAndCapitals"]),
  },
  created() {
    // this.getWeatherData();
    // this.getCovid();
    // this.getHotels();
    this.getPopulationArea();
  },

  data() {
    return {
      weatherApi: "a04401ce5a86509d82dbf7cafc6d2e6f",
      weatherUrl: "https://api.openweathermap.org/data/2.5/weather?q=",
      weatherData: null,
      units: "metric",
      //Covid data:
      covidUrl: "https://covid2019-api.herokuapp.com/country/",
      covidResults: null,
      countryCovid: this.showCountryCapital.country,

      //Population data:
      populationURl: "https://spott.p.rapidapi.com/places/",
      url2: "https://spott.p.rapidapi.com/places/",
      countryId: this.showCountryCapital.abbrev,
      populationArea: null,
    };
  },
  methods: {
    // GET POPULATION AND AREA REQUEST( deluje)
    getPopulationArea() {
      const options = {
        method: "GET",
        // url: `${this.url2}${this.countryId}`,
        url: `https://spott.p.rapidapi.com/places/${this.countryId}`,
        headers: {
          "x-rapidapi-host": "spott.p.rapidapi.com",
          "x-rapidapi-key":
            "c90dc18f0bmsh5e0e2cf7a230c4ep1b4723jsn4bbafe8dc12a",
        },
      };

      axios
        .request(options)
        .then((response) => {
          this.populationArea = response;
          console.log(this.populationArea);
        })
        .catch(function (error) {
          console.error(error);
        });
    },

    // GET WEATHER REQUEST : (deluje)
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
        console.log("GET WEATHER REQUEST:");
        console.log(this.weatherData);
      }
    },

    // GET COVID REQUEST : (deluje)
    async getCovid() {
      let response = await axios.get(`${this.covidUrl}${this.countryCovid}`);
      this.covidResults = response.data;
      // console.log("GET COVID REQUEST:");
      // console.log(this.covidResults);
    },

    // GET HOTELS REQUEST : (kliče vendar ne prikaže)
    // getHotels() {
    //   const options = {
    //     method: "GET",
    //     url: "https://hotels4.p.rapidapi.com/locations/v2/search",
    //     params: {
    //       query: this.showCountryCapital,
    //       locale: "en_US",
    //       currency: "USD",
    //     },
    //     headers: {
    //       "x-rapidapi-host": "hotels4.p.rapidapi.com",
    //       "x-rapidapi-key":
    //         "8e3eb0b54fmshbff78dad8e4074cp1bd8ccjsndca5d0654ae3",
    //     },
    //   };

    //   axios
    //     .request(options)
    //     .then(function (response) {
    //       this.hotels = response;
    //       console.log("GET HOTEL REQUEST");
    //       console.log(response);
    //     })
    //     .catch(function (error) {
    //       //   console.error(error);
    //     });
    //},
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&display=swap");

body {
  padding: 0;
  margin: 0;
  background-color: #17141d;
  color: white;
  font-family: "DM Mono", monospace;
}

#show .card-list {
  display: flex;
  padding: 3rem;
  overflow-x: scroll;
}

#show .card {
  display: flex;
  position: relative;
  flex-direction: column;
  height: 350px;
  width: 400px;
  min-width: 250px;
  padding: 1rem;
  border-radius: 16px;
  background: #17141d;
  box-shadow: -1rem 0 3rem #000;

  transition: 0.2s;
}

#show .card-list::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}
#show .card-list::-webkit-scrollbar-thumb {
  background: #201c29;
  border-radius: 10px;
  box-shadow: inset 2px 2px 2px hsla(0, 0%, 100%, 0.25),
    inset -2px -2px 2px rgba(0, 0, 0, 0.25);
}
#show .card-list::-webkit-scrollbar-track {
  background: linear-gradient(90deg, #201c29, #201c29 1px, #17141d 0, #17141d);
}

#show .card:hover {
  transform: translateY(-1rems);
}
#show .card:hover ~ .card {
  transform: translateX(130px);
}

#show .card::not(:first-child) {
  margin-left: -130px;
}

#show .card-author {
  position: relative;
  display: grid;
  grid-template-columns: 75px 1fr;
  align-items: center;
  margin: 3rem 0 0;
}

#show .author-avatar img {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  /* filter: grayscale(100%);  Uncomment for BLACK AND WHITE picture */
  margin: 16px 10px;
}

#show .half-circle {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 60px;
  height: 48px;
  fill: none;
  stroke: #ff8a00;
  stroke-width: 8;
  stroke-linecap: round;
}

#show .author-name-prefix {
  font-style: normal;
  font-weight: 700;
  color: #7a7a8c;
}

#show .card-header h2:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
