<template>
  <div class="container">
    <LoginForm />
  </div>
</template>

<script>
// @ is an alias to /src
import LoginForm from "@/components/Login/Login.vue";
export default {
  name: "Login",
  components: {
    LoginForm,
  },
};
</script>
