<template>
  <div class="home">
    <NavbarSite />
    <h1>Home</h1>
  </div>
</template>

<script>
// @ is an alias to /src
import NavbarSite from "@/components/NavbarSite.vue";

export default {
  name: "HomeView",
  components: {
    NavbarSite,
  },
};
</script>
