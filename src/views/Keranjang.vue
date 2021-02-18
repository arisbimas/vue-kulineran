<template>
  <div class="kerajang">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <div class="row mt-4">
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
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
                      :src="
                        require('../assets/images/' + keranjang.products.gambar)
                      "
                      alt=""
                      class="img-fluid shadow rounded"
                      width="250px"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td align="right">{{ keranjang.products.harga }}</td>
                  <td align="right">
                    <strong>{{
                      keranjang.products.harga * keranjang.jumlah_pemesanan
                    }}</strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      @click="hapusKeranjang(keranjang.id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga: </strong>
                  </td>
                  <td align="right">
                    <strong>{{ "Rp." + totalHarga }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <div class="row justify-content-end">
        <form action="" class="mt-4" v-on:submit.prevent>
          <div class="form-group">
            <label for="nama">Nama :</label>
            <input
              type="text"
              name="nama"
              class="form-control"
              v-model="pesan.nama"
            />
          </div>
          <div class="form-group">
            <label for="no_meja">No Meja :</label>
            <input
              type="text"
              name="no_meja"
              class="form-control"
              v-model="pesan.noMeja"
            />
          </div>

          <button type="submit" class="btn btn-success" @click="checkout">
            <b-icon-cart></b-icon-cart>Pesan
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },
  methods: {
    setKeranjang(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() =>
          this.$toast.success("Berhasil dihapus dari keranjang.", {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissable: true,
          })
        )
        .catch((error) => console.log(error));
    },
    checkout() {
      if (this.pesan.nama && this.pesan.noMeja) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            //hapus keranjang
            this.keranjangs.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.log(error));
            });

            //ridirect
            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Berhasil dipesan.", {
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
        this.$toast.error("Nama dan Nomer Meja Harus diisi.", {
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
      .get("http://localhost:3000/keranjangs/")
      .then((response) =>
        // handle success
        this.setKeranjang(response.data)
      )
      .catch((error) => console.log(error));
  },
  updated() {
    axios
      .get("http://localhost:3000/keranjangs/")
      .then((response) =>
        // handle success
        this.setKeranjang(response.data)
      )
      .catch((error) => console.log(error));
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