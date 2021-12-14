<template>
  <h1>home</h1>
  <!-- <p>
    User searching on this site comes from : {{ this.userIp.continent.code }},
    {{ this.userIp.country.name }} -
    {{ this.userIp.postcode }}
    {{ this.userIp.area.name }}
  </p> -->
  <div v-for="countryCapital in countriesAndCapitals" :key="countryCapital">
    <ShowCountries :showCountryCapital="countryCapital" />
  </div>
</template>

<script>
import axios from "axios";
import { mapState } from "vuex";
import ShowCountries from "../components/ShowCountries.vue";
// @ is an alias to /src

export default {
  name: "Home",
  components: {
    ShowCountries,
  },
  computed: {
    ...mapState(["countriesAndCapitals"]),
  },
  created() {
    // this.getIp();
  },
  data() {
    return {
      // IP data:
      ipUrl: "https://api.getgeoapi.com/v2/ip/check?api_key=",
      ipApi: "e3bb0ec8b7180a189fbde6d6339b4e394ebebea3",
      userIp: null,
    };
  },
  methods: {
    //IP REQUEST: (deluje)
    async getIp() {
      let res = await axios.get(`${this.ipUrl}${this.ipApi}&format=json`);
      this.userIp = res.data;
      // console.log(this.userIp);
    },
  },
};
</script>

<style></style>
