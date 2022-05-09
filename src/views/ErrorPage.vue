<template>
  <div>
    <img src="../assets/err.jpg" class="rounded mx-auto d-block h400" alt="" />
    <div class="text-center">
      <router-link :to="{ name: 'Home' }">
        <button class="btn btn-info">Visit Home Page</button></router-link
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "ErrorPage",
};
</script>

<style>
.h400 {
  height: 400px;
}
</style>