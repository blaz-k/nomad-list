<template>
  <div></div>
</template>

<script>
export default {
  name: "WeatherData",
  created() {
    this.weatherData();
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
    async getWeatherData() {
      let res = await axios.get(
        `${this.weatherUrl}London&appid=${this.weatherApi}`
      );
      weatherData = res.data;
      console.log(this.weatherData);
    },
  },
};
</script>

<style></style>
