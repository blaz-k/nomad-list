<template>Covid request component</template>

<script>
import axios from "axios";
export default {
  name: "Covid",
  props: ["showCountryCapital"],
  created() {
    this.getCovid();
  },

  data() {
    return {
      //Covid data:
      covidUrl: "https://covid2019-api.herokuapp.com/country/",
      covidResults: null,
      countryCovid: this.showCountryCapital.country,
    };
  },
  methods: {
    async getCovid() {
      let response = await axios.get(`${this.covidUrl}${this.countryCovid}`);
      this.covidResults = response.data;
      //   console.log(this.covidResults);
    },
  },
};
</script>

<style></style>
