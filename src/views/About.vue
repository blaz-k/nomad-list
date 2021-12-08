<template>
  <div id="about">
    <section class="card-list">
      <div v-for="countryCity in countriesAndCapitals" :key="countryCity">
        <article class="card">
          <header class="card-header">
            <p>May 25th 2020</p>
            <h2>{{ countryCity.country }}</h2>
            <h3>{{ countryCity.city }}</h3>
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
      </div>
    </section>
  </div>
  <!-- 
  <div v-for="countryCity in countriesAndCapitals" :key="countryCity">
    {{ countryCity.country }} - {{ countryCity.city }}
  </div> -->
</template>

<script>
import { mapState } from "vuex";

export default {
  name: "About",
  computed: {
    ...mapState(["countriesAndCapitals"]),
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

#about .card-list {
  display: flex;
  padding: 3rem;
  overflow-x: scroll;
}

#about .card {
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
  margin: 3rem 0 0;
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

#about .card-header h2:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
