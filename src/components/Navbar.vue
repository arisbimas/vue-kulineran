<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <b-navbar-brand href="#"
          ><img
            src="../assets/images/salad.png"
            class="img-fluid"
            width="50px"
          />
          Kulineran</b-navbar-brand
        >

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/foods">Foods</router-link>
            </li>
          </b-navbar-nav>

          <b-navbar-nav class="ml-auto">
            <li class="nav-item active">
              <router-link class="nav-link" to="/keranjang"
                >Keranjang <b-icon-bag></b-icon-bag
                ><span class="badge badge-success ml-2">{{
                  updateKeranjang
                    ? updateKeranjang.length
                    : this.jumlah_pesanan.length
                }}</span></router-link
              >
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  data() {
    return {
      jumlah_pesanan: [],
    };
  },
  props: ["updateKeranjang"],
  methods: {
    setJumlah(data) {
      this.jumlah_pesanan = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs/")
      .then((response) =>
        // handle success
        this.setJumlah(response.data)
      )
      .catch((error) => console.log(error));
  },
  updated() {
    axios
      .get("http://localhost:3000/keranjangs/")
      .then((response) =>
        // handle success
        this.setJumlah(response.data)
      )
      .catch((error) => console.log(error));
  },
};
</script>