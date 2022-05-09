<template>
  <Navbar />
  <form @click.prevent action="">
    <div class="row g-3 align-items-center">
      <div class="col-auto d-clock mx-auto">
        <input
          type="text"
          name=""
          class="form-control"
          placeholder="Restaurant Name"
          v-model="state.name"
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
          type="text"
          name=""
          class="form-control"
          placeholder="Phone Number"
          v-model="state.phone"
        />
        <span class="error-feedback" v-if="v$.phone.$error">
          {{ v$.phone.$errors[0].$message }}
        </span>
      </div>
    </div>
    <br />

    <div class="row g-3 align-items-center">
      <div class="col-auto d-clock mx-auto">
        <input
          type="text"
          name=""
          class="form-control"
          placeholder="Your Address"
          v-model="state.address"
        />
        <span class="error-feedback" v-if="v$.address.$error">
          {{ v$.address.$errors[0].$message }}
        </span>
      </div>
    </div>
    <div style="margin-top: 10px" class="row g-3 align-items-center">
      <div
        class="col-auto d-block mx-auto alert alert-success"
        v-if="successMsg.length > 0"
      >
        {{ successMsg }}
      </div>
      <div
        class="col-auto d-block mx-auto alert alert-danger"
        v-if="errorMsg.length > 0"
      >
        {{ errorMsg }}
      </div>
    </div>
    <div class="row g-3 align-items-center">
      <div class="col-auto d-clock mx-auto">
        <input
          type="submit"
          @click="addNewLocation()"
          class="btn btn-primary"
        />
        <span class="error-feedback" v-if="v$.address.$error">
          {{ v$.address.$errors[0].$message }}
        </span>
      </div>
    </div>
  </form>
</template>

<script>
import axios from "axios";
import { mapActions } from "vuex";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import { reactive, computed } from "vue";
import Navbar from "@/components/Header/Navbar.vue";

export default {
  name: "AddNewocation",
  setup() {
    // data
    const state = reactive({
      name: "",
      phone: "",
      address: "",
    });
    // validations
    const rules = computed(() => {
      return {
        name: { required }, // Matches this.pass
        phone: { required }, // Matches this.pass
        address: { required }, // Matches this.email
      };
    });
    const v$ = useVuelidate(rules, state);
    return {
      state,
      v$,
    };
  },
  data() {
    return {
      userId: "",
      successMsg: "",
      errorMsg: "",
    };
  },
  components: {
    Navbar,
  },
  mounted() {
    let user = localStorage.getItem("user-info");

    if (!user) {
      this.redirectTo({ val: "Signup" });
    } else {
      this.userId = JSON.parse(user).id;
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async addNewLocation() {
      this.v$.$validate(); // activite my validations
      if (!this.v$.$error) {
        let results = await axios.post("http://localhost:3000/locations", {
          name: this.state.name,
          phone: this.state.phone,
          address: this.state.address,
          userId: this.userId,
        });
        if (results.status == 201) {
          this.successMsg = "Added New Location";
          this.errorMsg = "";
          setTimeout(() => {
            this.redirectTo({ val: "Home" });
          }, 1500);
          localStorage.setItem("location-info", JSON.stringify(results.data));
        } else {
          console.log("error");
        }
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
  