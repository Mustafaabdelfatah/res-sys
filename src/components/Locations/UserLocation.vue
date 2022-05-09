<template>
  <div>
    <p>List Of Location ({{ allLocations.length }})</p>
    <table class="table" v-if="allLocations.length > 0">
      <thead class="table-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Phone</th>
          <th scope="col">Address</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(col, i) in allLocations" :key="i">
          <td>{{ i + 1 }}</td>
          <td>{{ col.name }}</td>
          <td>{{ col.phone }}</td>
          <td>{{ col.address }}</td>
          <td>
            <router-link
              :to="{ name: 'DeleteLocation', params: { LocationId: col.id } }"
            >
              <button class="btn btn-danger">Delete</button>
            </router-link>
            <router-link
              :to="{ name: 'UpdateLocation', params: { LocationId: col.id } }"
            >
              <button class="btn btn-info">Update</button>
            </router-link>
            <router-link :to="{ name: 'Menu', params: { locationId: col.id } }">
              <button class="btn btn-success">Menu</button>
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-else class="alert alert-warning" role="alert">
      No Location Added Yet
    </div>
  </div>
</template>

<script>
export default {
  name: "UserLocation",
  props: ["allLocations"],
};
</script>

<style>
</style>