<template>
  <div class="cart">
    <NavbarSite :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- breadcrumb   -->
      <div class="row mt-4">
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
                <li class="breadcrumb-item active" aria-current="page">Cart</li>
              </ol>
            </nav>
          </div>
        </div>
      </div>
      <!-- end breadcrumb -->

      <div class="container">
        <div class="row">
          <div class="col">
            <h2>My <strong>Cart</strong></h2>
            <div class="table-responsive mt-3">
              <table class="table table-hover">
                <thead>
                  <tr>
                    <th scope="col">No</th>
                    <th scope="col">Picture</th>
                    <th scope="col">Food</th>
                    <th scope="col">Information</th>
                    <th scope="col">Quantity</th>
                    <th scope="col">Price</th>
                    <th scope="col">Total Price</th>
                    <th scope="col">Delete</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(keranjang, index) in keranjangs"
                    :key="keranjang.id"
                  >
                    <th scope="row">{{ index + 1 }}</th>
                    <td>
                      <img
                        :src="'../assets/images/' + keranjang.products.gambar"
                        class="img-fluid shadow"
                        width="250"
                      />
                    </td>
                    <td>
                      <strong>{{ keranjang.products.nama }}</strong>
                    </td>
                    <td>
                      {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                    </td>
                    <td>{{ keranjang.jumlah_pemesanan }}</td>
                    <td align="right">Rp{{ keranjang.products.harga }}</td>
                    <td align="right">
                      <strong
                        >Rp{{
                          keranjang.products.harga * keranjang.jumlah_pemesanan
                        }}</strong
                      >
                    </td>
                    <td align="center" class="text-danger">
                      <b-icon-trash
                        @click="hapusKeranjang(keranjang.id)"
                      ></b-icon-trash>
                    </td>
                  </tr>

                  <tr>
                    <td colspan="6" align="right">
                      <strong>Total Price : </strong>
                    </td>
                    <td align="right">
                      <strong>Rp{{ totalHarga }}</strong>
                    </td>
                    <td></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>

        <!-- Form Checkout -->
        <div class="container">
          <div class="row justify-content-end">
            <form class="mt-4" v-on:submit.prevent>
              <div class="form-group">
                <label for="nama">Nama : </label>
                <input type="text" class="form-control" v-model="pesan.nama" />
              </div>
              <div class="form-group">
                <label for="noMeja">Nomor Meja : </label>
                <input
                  type="text"
                  class="form-control"
                  v-model="pesan.noMeja"
                />
              </div>
              <button
                type="submit"
                class="btn btn-success float-right"
                @click="checkout"
              >
                <b-icon-cart></b-icon-cart> Pesan
              </button>
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
  name: "CartView",
  components: {
    NavbarSite,
  },

  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },

  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$swal.fire({
            // optional options Object
            icon: "error",
            title: "Sukses",
            text: "Data Pesanan Berhasil Dihapus",
          });

          // Update Data Keranjang
          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log("Gagal", error));
        })
        .catch((error) => console.log("Gagal", error));
    },

    checkout() {
      if (this.pesan.nama && this.pesan.noMeja) {
        this.pesan.keranjangs = this.keranjangs;

        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            this.keranjangs.map(function (item) {
              // Hapus semua keranjang
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.log("Gagal", error));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$swal("Sukses Dipesan", {
              // optional options Object
              toast: true,
              position: "top-end",
              showConfirmButton: false,
              timer: 3000,
              icon: "success",
              title: "Hi from Sweetalert",
              text: "Have a good day ahead!",
              showCancelButton: "true",
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$swal.fire({
          // optional options Object
          icon: "error",
          title: "Wajib Diisi",
          text: "Nama dan Nomor Meja",
        });
      }
    },
  },

  mounted() {
    // Make a request for a user with a given ID
    // let instance = app.$toast.open('Basic Toast');
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log("Gagal", error));
  },

  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>