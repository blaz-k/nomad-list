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
            <p>May 25th 2020</p>
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
              <p>May 25th 2020</p>
              <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Country capital is: </span>
                <span class="badge-right">{{
                  showCountryCapital.city
                }}</span></span
              >

              <!-- <span class="badge rounded-pill bg-none">
                <span class="badge-left"> Confirmed Covid19 cases:</span>
                <span class="badge-right">
                  {{ covidResults[countryCovid].confirmed }}</span
                ></span
              > -->

              <span class="badge rounded-pill bg-none">
                <p>
                  <!-- There has been
                  {{ covidResults[countryCovid].confirmed }} confirmed casses
                  since the start of covid. -->
                </p></span
              >
              <!-- <span class="badge rounded-pill bg-none">
                <p>
                  it has population of {{ populationArea.population }} on
                  {{ populationArea.areaSqKm }} km/2
                </p></span
              > -->
            </header>
            <div class="card-author">
              <div class="author-name">
                <div class="author-name-prefix">Author</div>
                Blaz Kmetic
              </div>
            </div>
          </article>
        </div>
      </section>
    </div>
  </div>

  <!-- <div id="show">
    <div
      @click="cardOne == 'start' ? (cardOne = 'flipped') : (cardOne = 'start')"
      v-bind:class="{ flipme: cardOne == 'flipped' }"
    > -->
  <!-- <section
        class="card-list card__face card__face--front card"
        v-if="weatherData"
      >
        <article class="card">
          <header class="card-header"> -->
  <!-- <p>{{ hotels.data.suggestions[2].entities[0].name }}</p> -->
  <!-- <p>May 25th 2020</p>
            <h2>
              Temperature in {{ showCountryCapital.city }},
              {{ showCountryCapital.country }} is:
            </h2>
            <h3>{{ Math.round(weatherData.main.temp) }}°C</h3>
            <p> -->
  <!-- There has been {{ covidResults[countryCovid].confirmed }} confirmed
            casses since the start of covid. it has population of
            {{ populationArea.population }} on
            {{ populationArea.areaSqKm }} km/2 -->
  <!-- </p>
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
  </div>-->
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
    this.getWeatherData();
    // this.getCovid();
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

<style scoped>
.flipme {
  transform: rotateY(180deg);
}
body {
  font-family: sans-serif;
  color: rgb(229, 247, 217);
}

.card {
  transition: transform 1s ease;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.is-flipped {
  transform: rotateY(180deg);
}

/* nisem */
.card__face {
  position: absolute;
  width: 100%;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
}

.card__face--back {
  transform: rotateY(180deg);
  backface-visibility: hidden;
}

/* @import url("https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&display=swap"); */

/* body {
  padding: 0;
  margin: 0;
  background-color: #17141d;
  color: white;
  font-family: "DM Mono", monospace;
} */

#about .card-list {
  display: flex;
  padding: 0.5rem;
  /* overflow-x: scroll; */
}

#about .card {
  display: flex;
  position: relative;
  flex-direction: column;
  height: 350px;
  width: 400px;
  min-width: 150px;
  padding: 0.9rem;
  border-radius: 26px;
  background: #1d1413;
  box-shadow: -1rem 0 4em rgb(184, 2, 2);
  /* margin-left: 50px; */

  transition: 0.2s;
}

/*Change card background color when hovering */
#about .card__face--front .card:hover {
  background: linear-gradient(45deg, rgb(128, 255, 156), rgb(5, 55, 163));
}

#about .card__face--back .card:hover {
  background: linear-gradient(45deg, #ff8a00, #e52e71);
}

#about .card-list::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}
#about .card-list::-webkit-scrollbar-thumb {
  background: #201c29;
  border-radius: 10px;
  box-shadow: inset 2px 2px 2px hsla(0, 0%, 100%, 0.25),
    inset -2px -2px 2px rgba(0, 0, 0, 0.25);
}
#about .card-list::-webkit-scrollbar-track {
  background: linear-gradient(90deg, #201c29, #201c29 1px, #17141d 0, #17141d);
}

#about .card:hover {
  transform: translateY(-1rems);
  transition: 1.5s linear;
  margin-left: -5%;
}
#about .card:hover ~ .card {
  transform: translateX(130px);
}

#about .card::not(:first-child) {
  margin-left: -130px;
}

#about .card-author {
  position: relative;
  display: grid;
  grid-template-columns: 75px 1fr;
  align-items: center;
  margin: 9rem 0 0;
}

#about .author-avatar img {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  /* filter: grayscale(100%);  Uncomment for BLACK AND WHITE picture */
  margin: 16px 10px;
}

#about .half-circle {
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

#about .author-name-prefix {
  font-style: normal;
  font-weight: 700;
  color: #7a7a8c;
}

#about .card__face--front .card-header h2:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Lahko uporabim oranzno /rdec hover na sprednji strani in ta spodnji hover na zadnji strani čez cel background */
#about .card__face--back .badge:hover {
  background: linear-gradient(30deg, rgb(128, 255, 156), rgb(5, 55, 163));
  text-shadow: none;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* BADGE */

.badge {
  border: seagreen;
  border-style: outset;
  display: flex;
  margin-bottom: 4px;
}

.badge-left {
  font-weight: 300;
  font-size: 1rem;
}
.badge-right {
  font-weight: 900;
  font-size: 1rem;
  margin-left: 15px;
}
</style>
