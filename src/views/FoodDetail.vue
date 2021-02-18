<template>
  <div class="food-detail">
    <Navbar />

    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-2">
        <div class="col-md-6">
          <img
            :src="loadGambar.gambar"
            alt=""
            class="img-fluid shadow rounded"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga : <strong>{{ "Rp. " + product.harga }}</strong>
          </h4>
          <form action="" class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pesan">Jumlah Pesan</label>
              <input
                type="number"
                name="jumlah_pesan"
                class="form-control"
                v-model="pesan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                name="keterangan"
                class="form-control"
                placeholder="Keterangan spt: Pedes dll"
                v-model="pesan.keterangan"
              />
            </div>
            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart>Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlah_pemesanan) {
        this.pesan.products = this.product;
        axios
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            //ridirect ke keranjang
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Berhasil masuk keranjang.", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissable: true,
            });
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        this.$toast.error("Jumlah pesanan harus diisi.", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissable: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) =>
        // handle success
        this.setProduct(response.data)
      )
      .catch((error) => console.log(error));
  },
  computed: {
    loadGambar() {
      return {
        ...this.product,
        gambar:
          this.product.gambar &&
          require(`../assets/images/${this.product.gambar}`),
      };
    },
  },
};
</script>

<style>
</style>