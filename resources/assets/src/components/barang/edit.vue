<template>
  <div class="container p-3">
    <div class="row justify-content-center">
      <div class="col-md-12">
        <div class="card card-default">
          <div class="card-header">Edit Barang</div>

          <div class="card-body">
            <form @submit.prevent="BarangUpdate">
              <!-- 'nama_barang', 'kode_barang', 'detail_barang', 'kategori_id', 'fungsi', 'harga_barang', 'lokasi', 'user_id' -->
              <div class="form-group">
                <label for="nama_barang">Nama Barang</label>
                <input
                  type="text"
                  name="nama_barang"
                  class="form-control"
                  v-model="barang.nama_barang"
                  placeholder="Masukan Nama Barang"
                />
              </div>

              <div class="form-group">
                <label for="kode_barang">Kode Barang</label>
                <input
                  type="text"
                  name="kode_barang"
                  class="form-control"
                  v-model="barang.kode_barang"
                  placeholder="Masukan Kode Barang"
                />
              </div>

              <div class="form-group">
                <label for="detail_barang">Detail Barang</label>
                <textarea
                  name="detail_barang"
                  class="form-control"
                  v-model="barang.detail_barang"
                  placeholder="Masukan Detail Barang"
                ></textarea>
              </div>

              <div class="form-group">
                <label for="kategori_id">Kategori</label>
                <select
                  name="kategori_id"
                  class="form-control"
                  v-model="barang.kategori_id"
                >
                  <option value="" disabled>Pilih Kategori</option>
                  <option
                    v-for="kategori in kategori"
                    :value="kategori.id"
                    :key="kategori.id"
                  >
                    {{ kategori.nama_kategori }}
                  </option>
                </select>
              </div>

              <div class="form-group">
                <label for="fungsi">Fungsi</label>
                <input
                  type="text"
                  name="fungsi"
                  class="form-control"
                  v-model="barang.fungsi"
                  placeholder="Masukan Fungsi"
                />
              </div>

              <div class="form-group">
                <label for="harga_barang">Harga Barang</label>
                <input
                  type="text"
                  name="harga_barang"
                  class="form-control"
                  v-model="barang.harga_barang"
                  placeholder="Masukan Harga Barang"
                />
              </div>

              <div class="form-group">
                <label for="lokasi_id">Lokasi</label>
                <select
                  name="lokasi_id"
                  class="form-control"
                  v-model="barang.lokasi_id"
                >
                  <option value="" disabled>Pilih Lokasi</option>
                  <option
                    v-for="lokasi in lokasi"
                    :value="lokasi.id"
                    :key="lokasi.id"
                  >
                    {{ lokasi.lokasi }}
                  </option>
                </select>
              </div>

              <div class="form-group">
                <label for="jenis_id">Jenis</label>
                <select
                  name="jenis_id"
                  class="form-control"
                  v-model="barang.jenis_id"
                >
                  <option value="" disabled>Pilih Jenis</option>
                  <option
                    v-for="jenis in jenis"
                    :value="jenis.id"
                    :key="jenis.id"
                  >
                    {{ jenis.jenis_barang }}
                  </option>
                </select>
              </div>

              <div class="form-group">
                <label for="user_id">User</label>
                <select
                  name="user_id"
                  class="form-control"
                  v-model="barang.user_id"
                >
                  <option value="" disabled>Pilih User</option>
                  <option v-for="user in user" :key="user.id" :value="user.id">
                    {{ user.name }}
                  </option>
                </select>
              </div>

              <div class="form-group">
                <button class="btn btn-md btn-success" type="submit">
                  SIMPAN
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      barang: {
        user: {},
        kategori: {},
      },
      kategori: [],
      user: [],
      lokasi: [],
      jenis: [],

    };
  },
  created() {
    axios
      .get("/api/kategori")
      .then((response) => {
        this.kategori = response.data.kategori;
      })
      .catch((errors) => {
        console.log(errors);
      });
    axios
      .get("/api/barang/" + this.$route.params.id)
      .then((response) => {
        this.barang = response.data.barang;
      })
      .catch((errors) => {
        console.log(errors);
      });

    axios.get("/api/kategori").then((response) => {
      this.kategori = response.data.kategori;
    });
    axios.get("/api/users").then((response) => {
      this.user = response.data.user;
    });
    axios.get("/api/jenis").then((response) => {
      this.jenis = response.data.jenis;
    });
    axios.get("/api/lokasi").then((response) => {
      this.lokasi = response.data.lokasi;
    });
  },
  methods: {
    BarangUpdate() {
      let uri = "/api/barang/" + this.$route.params.id;
      axios.put(uri, this.barang).then((response) => {
        this.$router.push("/barang");
      });
    },
  },
};
</script>
