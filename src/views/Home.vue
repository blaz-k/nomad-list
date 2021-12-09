<template>
  <h1>home</h1>
  <div v-for="countryCapital in countriesAndCapitals" :key="countryCapital">
    <ShowCountries :showCountryCapital="countryCapital" />
    <!-- <ShowCountries :showCountryCapital="countryCapital" /> -->
  </div>
</template>

<script>
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
};
</script>

<style></style>
