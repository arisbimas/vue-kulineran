<template>
  <div class="pesanan-sukses">
    <Navbar />
    <div class="container mt-4">
      <div class="row justify-content-center">
        <div class="col text-center">
          <img src="../assets/images/menunggu.png" width="350" />
          <h2>Sukses</h2>
          <h4>
            Pesanan Segera diproses <br />
            Silahkan Tunggu
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "PesananSukses",
  components: {
    Navbar,
  },
};
</script>

<style>
</style>