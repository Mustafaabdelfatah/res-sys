<template>
  <ul id="nav">
    <router-link
      tag="li"
      exact
      :to="{ name: 'Home', params: { pageTitle: 'Home Page' } }"
    >
      <a href=""> Home</a>
    </router-link>
    <a href="#" @click="logOut()">Logout</a>
    <router-link
      tag="li"
      exact
      :to="{ name: 'Profile', params: { pageTitle: 'Profile Page' } }"
    >
      <a href=""> Profile</a>
    </router-link>
  </ul>
</template>

<script>
import { mapActions } from "vuex";

export default {
  name: "Navbar",

 
  methods: {
    ...mapActions(["redirectTo"]),

    logOut() {
      localStorage.clear();
      this.redirectTo({ val: "Login" });
    },
  },
};
</script>

 <style lang="scss" scoped>
$fontColor: darkcyan;
$navColor: #333;
ul {
  display: flex;
  justify-content: center;
  list-style: none;
  margin-bottom: 50px;
}
a {
  text-decoration: none;
  font-weight: bold;
  color: $fontColor;
  padding: 10px;
}
</style>