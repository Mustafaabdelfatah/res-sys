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
            placeholder="Restaurant Name"
            v-model="name"
          />
        </div>
      </div>
      <br />

      <div class="row g-3 align-items-center">
        <div class="col-auto d-clock mx-auto">
          <input
            type="text"
            name=""
            class="form-control"
            placeholder="phone"
            v-model="phone"
          />
        </div>
      </div>
      <br />

      <div class="row g-3 align-items-center">
        <div class="col-auto d-clock mx-auto">
          <input
            type="text"
            name=""
            class="form-control"
            placeholder="Address"
            v-model="address"
          />
        </div>
      </div>
      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-clock mx-auto">
          <button type="submit" @click="UpdLocation()" class="btn btn-primary">
            Update Profile Now
          </button>
        </div>
      </div>
      <div
        class="col-auto d-block mx-auto alert alert-info"
        v-if="successMsg.length > 0"
      >
        {{ successMsg }}
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
  name: "UpdateLoaction",
  data() {
    return {
      name: "",
      phone: "",
      address: "",
      LocationId: "",
      userId: "",
      successMsg: "",
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (!user) {
      this.redirectTo({ val: "Signup" });
    } else {
      // لو في يوزر
      this.LocationId = this.$route.params.LocationId;
      this.userId = JSON.parse(user).id;
      // console.log(this.userId);
      // console.log(this.LocationId);
      this.canCurrentUserAccessThisLocation();
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async canCurrentUserAccessThisLocation() {
      let results = await axios.get(
        `http://localhost:3000/locations?id=${this.LocationId}&userId=${this.userId}`
      );
      console.log(results);
      if (results.status == 200 && results.data.length > 0) {
        this.LocationData = results.data;
        this.name = this.LocationData[0].name;
        this.address = this.LocationData[0].address;
        this.phone = this.LocationData[0].phone;
      } else {
        this.redirectTo({ val: "Home" });
      }
    },
    async UpdLocation() {
      let results = await axios.put(
        `http://localhost:3000/locations/${this.LocationId}`,
        {
          name: this.name,
          phone: this.phone,
          address: this.address,
          userId: this.userId,
        }
      );

      if (results.status == 200) {
        this.successMsg = "updated Succeed";
        setTimeout(() => {
          this.redirectTo({ val: "Home" });
        }, 2000);
      } else {
        console.log("wrong");
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
  