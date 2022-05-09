<template>
  <div class="home"></div>
  <Navbar />

  <div class="container">
    <div class="row">
      <p class="text-end">
        Welcome
        <button
          style="
            width: 139px;
            background: #18979e;
            height: 40px;
            border-radius: 4px;
            outline: 0;
            color: #fff;
            text-transform:capitalize
          "
        >
          {{ userName }}
        </button>
      </p>

 

<router-link :to="{name:'AddNewLocation'}">
   <button
      type="button"
      class="btn btn-primary"
      
    >
      Add New Restaurant
    </button>
</router-link>
    

      <!-- <AddNewLocation /> -->

      <UserLocation :allLocations="listOfLocations" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { mapActions } from "vuex";
import Navbar from "@/components/Header/Navbar.vue";
import UserLocation from "@/components/Locations/UserLocation.vue";
// import AddNewLocation from "@/components/Locations/AddNewLocation.vue";
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      userName: "",
      userId: "",
      listOfLocations: [],
    };
  },
  components: {
    Navbar,
   
    UserLocation,
  },
  async mounted() {
    let user = localStorage.getItem("user-info");
    console.log(user);
    if (!user) {
      this.redirectTo({ val: "Signup" });
    } else {
      this.userName = JSON.parse(user).name;
      this.userId = JSON.parse(user).id;
    }
    let results = await axios.get(
      `http://localhost:3000/locations?userId=${this.userId}`
    );
    if (results.status == 200 && results.data.length > 0) {
      this.listOfLocations = results.data;
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
  },
};
</script>
