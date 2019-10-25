<template>
  <div id="app" class="bg-black">
    <Homepage/>
    <router-view></router-view>

    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>

    <section v-else>
      <ul>
        <li v-if="loading">Loading...</li>

        <li v-else v-for="user in users" :key="user.id">
          <span class="flex flex-row white">{{user.first_name}}</span>
          
          <img :src="user.avatar" alt class="src">
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import Homepage from "./components/Homepage";
import axios from "axios";

export default {
  name: "App",
  components: {
    Homepage
  },
  data() {
    return {
      users: null,
      loading: true,
      errored: false
    };
  },
  mounted() {
    axios
      .get("//reqres.in/api/users/2")
      .then(response => {
        console.log(response.data);
        this.users = response.data;
      })
      .catch(error => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  }
};
</script>

<style>
@import './assets/css/style.css';
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  list-style-type: none;
}
</style>
