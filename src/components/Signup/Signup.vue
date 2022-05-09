<template>
  <form @click.prevent action="">
    <div class="row g-3 align-items-center">
      <h1>Sign Up</h1>
      <div class="col-auto d-clock mx-auto">
        <input
          type="text"
          name=""
          class="form-control"
          placeholder="Enter Your Name"
          v-model="name"
        />
        <span class="error-feedback" v-if="v$.name.$error">
          {{ v$.name.$errors[0].$message }}
        </span>
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
        <span class="error-feedback" v-if="v$.email.$error">
          {{ v$.email.$errors[0].$message }}
        </span>
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
        <span class="error-feedback" v-if="v$.pass.$error">
          {{ v$.pass.$errors[0].$message }}
        </span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-clock mx-auto">
        <button type="submit" @click="signUpNow()" class="btn btn-primary">
          Sign Up Now
        </button>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <button
          type="button"
          @click="redirectTo({ val: 'Login' })"
          class="btn btn-primary"
        >
          Login
        </button>
      </div>
    </div>
  </form>
</template>
 
 <script>
import axios from "axios";
import { mapActions } from "vuex";
import useVuelidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";

export default {
  name: "SignUpForm",
  data() {
    return {
      v$: useVuelidate(),
      name: "",
      pass: "",
      email: "",
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.redirectTo({ val: "Home" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async signUpNow() {
      this.v$.$validate();

  

      if (!this.v$.$error) {
        let results = await axios.post("http://localhost:3000/users", {
          name: this.name,
          email: this.email,
          pass: this.pass,
        });

  console.log(results);
        if (results.status == 201) {
          console.log("success");
          localStorage.setItem("user-info", JSON.stringify(results.data));
          this.redirectTo({ val: "Home" });
        } else {
          console.log("error");
        }
      } else {
        console.log("fail");
      }
    },
  },
  validations() {
    return {
      name: { required }, // Matches this.firstName
      pass: { required }, // Matches this.pass
      email: { required, email }, // Matches this.email
    };
  },
};
</script>
 
 <style scoped>
.error-feedback {
  color: red;
  font-size: 0.85em;
}
</style>
  