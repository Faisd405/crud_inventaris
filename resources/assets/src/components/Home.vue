<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-12">
        <div class="card card-default">
          <div class="card-header">Home</div>

          <div class="card-body mx-auto">
            <div class="row">
              <div
                class="card text-white bg-primary col-md-6"
                style="max-width: 18rem"
              >
                <div class="card-header">Total Barang</div>
                <div class="card-body">
                  <h1 class="card-title">{{ barang.length }}</h1>
                </div>
              </div>
              <div
                class="card text-white bg-primary col-md-6"
                style="max-width: 18rem"
              >
                <div class="card-header">Total Users</div>
                <div class="card-body">
                  <h1 class="card-title">{{ user.length }}</h1>
                </div>
              </div>
              <div
                class="card text-white bg-primary col-md-6"
                style="max-width: 18rem"
              >
                <div class="card-header">Total Buku</div>
                <div class="card-body">
                  <h1 class="card-title">{{ buku.length }}</h1>
                </div>
              </div>
              <div
                class="card text-white bg-primary col-md-6"
                style="max-width: 18rem"
              >
                <div class="card-header">Total Barang Yang Terpakai</div>
                <div class="card-body">
                  <h1 class="card-title">
                    {{ barang.length - NoUser.length }}
                  </h1>
                </div>
              </div>
              <div
                class="card text-white bg-primary col-md-6"
                style="max-width: 18rem"
              >
                <div class="card-header">Total Barang Yang Tidak Terpakai</div>
                <div class="card-body">
                  <h1 class="card-title">{{ NoUser.length }}</h1>
                </div>
              </div>
              <div
                class="card text-white bg-primary col-md-6"
                style="max-width: 18rem"
              >
                <div class="card-header">Total Harga Barang</div>
                <div class="card-body">
                  <h1 class="card-title">{{ total }}</h1>
                </div>
              </div>
            </div>
            <div>
                <label>Chart Kategori :</label>
              <KategoriChart></KategoriChart>
                <label>Chart Total Aset Pertahun :</label>
              <TotalChart></TotalChart>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import KategoriChart from "./Chart/KategoriChart.vue";
import TotalChart from "./Chart/TotalChart.vue";
import axios from "axios";

export default {
  name: "home",
  metaInfo: {
    title: "Home",
  },
  components: {
    KategoriChart,
    TotalChart,
  },
  data() {
    return {
      barang: [],
      buku: [],
      user: [],
      NoUser: [],
      kategori: [],
      total: [],
    };
  },
  created() {
    axios.get(`/api/barang`).then((response) => {
      this.barang = response.data.barang;
    });
    axios.get(`/api/barang/NoUser`).then((response) => {
      this.NoUser = response.data.barang;
    });
    axios.get(`/api/barang/total`).then((response) => {
      this.total = response.data.total;
    });
    axios.get(`/api/buku`).then((response) => {
      this.buku = response.data.buku;
    });
    axios.get(`/api/users`).then((response) => {
      this.user = response.data.user;
    });
    axios.get(`/api/kategori`).then((response) => {
      this.kategori = response.data.kategori;
    });
  },
};
</script>
