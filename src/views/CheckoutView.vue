<template>
  <div class="pesanan-sukses">
    <NavbarSite />
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col text-center">
                <h2><strong>Success!!!</strong></h2>
                <img src="../assets/images/menunggu.png" width="300">
                <h4>
                    Pesanan Anda Segera Diproses <br>Selamat Menunggu
                </h4>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import NavbarSite from "@/components/NavbarSite.vue";

export default {
  name: "CheckoutView",
  components: {
    NavbarSite,
  },
};
</script>

<style>
</style>
