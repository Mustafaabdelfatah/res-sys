<template>
  <Navbar />

  <div class="container">
    <div class="text-center">
      <h1>Delete Restaurant # {{ LocationId }}</h1>
      <span style="color: #f00"
        >Are U Sure U Want To Delete This Restaurant ?</span
      >
      <hr />
      <p>Name : {{ name }}</p>
      <p>Address : {{ address }}</p>
      <p>Phone : {{ phone }}</p>
      <hr />
    </div>
    <div
        class="col-auto d-block mx-auto alert alert-danger"
        v-if="successMsg.length > 0"
      >
        {{ successMsg }}
      </div>
    <div
      class="text-center"
      style="display: flex; margin-top: 40px; justify-content: space-evenly"
    >
      <button @click="goBack()" class="btn btn-success">Go Back</button>
      <button @click="Delete()" class="btn btn-danger">Delete</button>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Header/Navbar.vue";
import { mapActions } from "vuex";
import axios from "axios";

export default {
  name: "DeleteLocation",
  data() {
    return {
      name: "",
      address: "",
      phone: "",
      userId: "",
      LocationId: "",
      LocationData: [],
       successMsg: "",
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
    goBack() {
      this.redirectTo({ val: "Home" });
    },
    async Delete() {
      let results = await axios.delete(
        `http://localhost:3000/locations/${this.LocationId}`
      );
      if (results.status == 200) {
           this.successMsg = "Deleted Succeed";
        setTimeout(() => {
          this.redirectTo({ val: "Home" });
        }, 2000);
      } else {
        console.log("wrong");
      }
    },
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
  },
};
</script>

<style>
</style>