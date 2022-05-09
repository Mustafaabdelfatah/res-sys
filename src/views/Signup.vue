<template>
  <div class="container">
    <SignUpForm />
  </div>
</template>

<script>
// @ is an alias to /src
import SignUpForm from "@/components/Signup/Signup.vue";
export default {
  name: "Signup",
  components: {
    SignUpForm,
  },
};
</script>
