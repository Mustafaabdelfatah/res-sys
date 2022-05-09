<template>
  <form @click.prevent action="">
    <div class="row g-3 align-items-center">
      <h1>Login</h1>
      <div class="col-auto d-clock mx-auto">
        <input
          type="email"
          name=""
          class="form-control"
          placeholder="Enter Your Email"
          v-model="state.email"
        />
         <span class="error-feedback" v-if="v$.email.$error"> {{v$.email.$errors[0].$message}} </span>

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
          v-model="state.pass"
        />
         <span class="error-feedback" v-if="v$.pass.$error"> {{v$.pass.$errors[0].$message}} </span>

      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-clock mx-auto">
        <button type="submit" @click="login()" class="btn btn-primary">Login</button>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <button
          type="button"
          @click="redirectTo({ val: 'Signup' })"
          class="btn btn-primary"
        >
          Sign Up
        </button>
      </div>
    </div>
  </form>
</template>
 
 <script>
import { mapActions } from "vuex";
import axios from "axios";
import useVuelidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";
import { reactive, computed } from "vue";
export default {
  name: "LoginForm",
  setup() {
    // data

    const state = reactive({
      pass: "",
      email: "",
    });

    // validations

    const rules = computed(() => {
      return {
        pass: { required }, // Matches this.pass
        email: { required, email }, // Matches this.email
      };
    });
    const v$ =  useVuelidate( rules , state);
    return {
      state , v$ ,
    };

  },

  data() {
    return {};
  },
    mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.redirectTo({ val: "Home" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
   async login(){
      this.v$.$validate();
      if(!this.v$.error){
        let results = await axios.get(`http://localhost:3000/users?email=${this.state.email}&pass=${this.state.pass}`);
       
        if(results.status == 200 && results.data.length > 0){
           
          localStorage.setItem("user-info", JSON.stringify(results.data[0]));
          this.redirectTo({ val: "Home" });
        }else{
          console.log('fail');
        }
        
      }else{
        console.log('fail');
      }
    }
  },
};
</script>
 
 <style scoped>
 .error-feedback{
   color: red;
   font-size: 0.85em;
 }
 </style>
  