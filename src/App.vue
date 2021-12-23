<template>
  <div id="nav">
    <router-link :to="{ name: 'Home' }">Home</router-link> |
    <router-link :to="{ name: 'About' }">About</router-link>
  </div>
  <router-view />
</template>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: sans-serif;

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #4e6174;
}
::selection {
  background-color: rgb(189, 243, 42);
}

#nav {
  margin-bottom: 6rem;
  padding: 30px;
  text-align: center;
  box-shadow: -1rem 60px 8em rgb(29, 106, 248);
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
  /* box-shadow: 0.4rem 0 2em rgb(124, 29, 248); */
  box-shadow: 0.4rem 0 1.5em rgb(184, 2, 2);
}

#nav a.router-link-exact-active {
  color: #42b983;
  /* box-shadow: 0.4rem 0 1em rgb(122, 10, 10); */
}
</style>
