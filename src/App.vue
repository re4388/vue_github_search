<template>
  <div id="app">
    <b-container>
      <header>
        <a href="https://github.com/">
          <img src="./assets/github_small.png" alt="Github small logo" />
        </a>
        <b-nav align="fixed-top" class="m-3">
          <b-nav-item active>
            <router-link :to="{ name: 'Hello' }">Home</router-link>
          </b-nav-item>
          <b-nav-item active>
            <router-link to="/about">About</router-link>
          </b-nav-item>
          <b-nav-item active>
            <router-link to="/repos">Repos Fast Look</router-link>
          </b-nav-item>
          <!-- <input v-model="userId" placeholder="edit me"> -->
        </b-nav>
      </header>
      <main>
        <router-view></router-view>
      </main>
    </b-container>
  </div>
</template>

<script>
// import dotenv from "dotenv";
// require('dotenv').config()


export default {
  name: "app",
  data() {
    return {
      // userId: ""
    };
  },
  mounted() {
    console.log(process.env.VUE_APP_GITHUB_USER_NAME);
    console.log(process.env.VUE_APP_GITHUB_PASSWROD);
    // console.log();
    
  },
  // watch: {
  //   $route: function() {
  //     this.userId = this.$router.history.current.params.id
  //   }
  // }
};
</script>

<style lang="scss" scoped>
html {
  height: 100%;
  background: #63d0ff;
}
body {
  font-family: "Montserrat", sans-serif;
  margin: 0;
  padding: 0;
  background: #63d0ff;
  height: 100%;
}
</style>