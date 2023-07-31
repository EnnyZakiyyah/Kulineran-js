<template>
  <div>
    <NavbarSite />
    <h2>Daftar Makanan</h2>
  </div>
</template>

<script>
// @ is an alias to /src
import NavbarSite from "@/components/NavbarSite.vue";

export default {
  name: "FoodsView",
  components: {
    NavbarSite,
  },
};
</script>
