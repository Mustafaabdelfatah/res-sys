<template>
  <div class="container">
    <ProfileForm />
  </div>
</template>

<script>
// @ is an alias to /src
import ProfileForm from "@/components/Profile/Profile.vue";
export default {
  name: "Profile",
  components: {
    ProfileForm,
  },
};
</script>
