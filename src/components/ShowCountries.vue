<template>
  <div v-if="weatherData">
    <p>
      Temperature in {{ showCountryCapital.country }},
      {{ showCountryCapital.city }} is: {{ weatherData.main.temp }}°C
    </p>
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
    this.getWeatherData();
  },
  data() {
    return {
      weatherApi: "a04401ce5a86509d82dbf7cafc6d2e6f",
      weatherUrl: "https://api.openweathermap.org/data/2.5/weather?q=",
      weatherData: null,
      units: "metric",
    };
  },
  methods: {
    //KAKO KLICAT COUNTY NAMESTO CITY ČE IMA CITY ERROR
    async getWeatherData() {
      let res = await axios.get(
        `${this.weatherUrl}${this.showCountryCapital.city}&units=${this.units}&appid=${this.weatherApi}`
      );

      //   if (res === "404") {
      //     let res = await axios.get(
      //       `${this.weatherUrl}${this.showCountryCapital.country}&units=${this.units}&appid=${this.weatherApi}`
      //     );
      //   } else {
      //     let res = await axios.get(
      //       `${this.weatherUrl}${this.showCountryCapital.city}&units=${this.units}&appid=${this.weatherApi}`
      //     );
      //   }
      if (res === "404 (Not Found)") {
        let res = await axios.get(
          `${this.weatherUrl}${this.showCountryCapital.country}&units=${this.units}&appid=${this.weatherApi}`
        );
        this.weatherData = res.data;
        console.log(this.weatherData);
      } else {
        this.weatherData = res.data;
        console.log(this.weatherData);
      }
    },
  },
};
</script>

<style></style>
