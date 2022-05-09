<template>
  <div class="container">
    <navbar></navbar>
    <form @click.prevent action="">
      <div class="row g-3 align-items-center">
        <h1 text-center>Update Profile</h1>
        <div class="col-auto d-clock mx-auto">
          <input
            type="text"
            name=""
            class="form-control"
            placeholder="Enter Your Name"
            v-model="name"
          />
        </div>
      </div>
      <br />

      <div class="row g-3 align-items-center">
        <div class="col-auto d-clock mx-auto">
          <input
            type="email"
            name=""
            class="form-control"
            placeholder="Enter Your Email"
            v-model="email"
          />
        </div>
      </div>
      <br />

      <div class="row g-3 align-items-center">
        <div class="col-auto d-clock mx-auto">
          <input
            type="password"
            name=""
            class="form-control"
            placeholder="Enter Your Password"
            v-model="pass"
          />
        </div>
      </div>
      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-clock mx-auto">
          <button type="submit" @click="UpdProfile()" class="btn btn-primary">
            Update Profile Now
          </button>
        </div>
      </div>
    </form>
  </div>
</template>
 
 <script>
import Navbar from "../Header/Navbar.vue";
import { mapActions } from "vuex";
import axios from "axios";

export default {
  components: { Navbar },
  name: "ProfileForm",
  data() {
    return {
      name: "",
      pass: "",
      email: "",
      userId: "",
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.name = JSON.parse(user).name;
      this.pass = JSON.parse(user).pass;
      this.email = JSON.parse(user).email;
      this.userId = JSON.parse(user).id;
    } else {
      this.redirectTo({ val: "Login" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async UpdProfile() {
      let results = await axios.put(
        `http://localhost:3000/users/${this.userId}`,
        {
          name: this.name,
          email: this.email,
          pass: this.pass,
        }
      );

      if (results.status == 200) {
        localStorage.setItem("user-info", JSON.stringify(results.data));

        this.redirectTo({ val: "Home" });
      } else {
        console.log("fail");
      }
    },
  },
};
</script>
 
 <style scoped>
.error-feedback {
  color: red;
  font-size: 0.85em;
}
</style>
  