<template>
  <div class="food-detail">
    <NavbarSite />
    <div class="container">
      <!-- breadcrumb   -->
      <div class="row mt-5">
        <div class="col">
          <div class="container">
            <nav aria-label="breadcrumb">
              <ol class="breadcrumb">
                <li class="breadcrumb-item">
                  <router-link to="/" class="text-dark">Home</router-link>
                </li>
                <li class="breadcrumb-item">
                  <router-link to="/foods" class="text-dark">Foods</router-link>
                </li>
                <li class="breadcrumb-item active" aria-current="page">
                  Food Order
                </li>
              </ol>
            </nav>
          </div>
        </div>
      </div>

      <div class="container">
        <div class="row mt-3">
          <div class="col-md-7">
            <img
              :src="'../assets/images/' + product.gambar"
              class="img-fluid shadow"
              alt=""
            />
          </div>
          <div class="col-md-5">
            <h2>
              <strong>{{ product.nama }}</strong>
            </h2>
            <hr />
            <h4>
              Harga: <strong>Rp{{ product.harga }}</strong>
            </h4>
            <form class="mt-4">
              <div class="form-group">
                <label for="jumlah_pesan">Jumlah Pesan</label>
                <input type="number" class="form-control" />
              </div>
              <div class="form-group">
                <label for="keterangan">Keterangan</label>
                <textarea
                  class="form-control"
                  placeholder="Keterangan seperti: Pedas, Nasi Setengah, dll ..."
                ></textarea>
              </div>
              <button type="submit" class="btn btn-success"><b-icon-cart></b-icon-cart> Pesan</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavbarSite from "@/components/NavbarSite.vue";
import axios from "axios";

export default {
  name: "FoodDetailView",
  components: {
    NavbarSite,
  },

  data() {
    return {
      product: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
  },

  mounted() {
    // Make a request for a user with a given ID
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log("Gagal", error));
  },
};
</script>

<style>
</style>