<template>
  <div class="container mt-3">
    <div class="row justify-content-center">
      <div class="col-md-12">
        <div class="card card-default">
          <div class="card-header">buku</div>

          <div class="card-body">
            <router-link
              :to="{ name: 'create-buku' }"
              class="btn btn-md btn-primary"
              >TAMBAH Data buku</router-link
            >
            <div class="table-responsive mt-2">
                <b-row>
                <b-col lg="6" class="my-1">
                  <b-form-group
                    label="Filter"
                    label-for="filter-input"
                    label-cols-sm="3"
                    label-align-sm="right"
                    label-size="sm"
                    class="mb-0"
                  >
                    <b-input-group size="sm">
                      <b-form-input
                        id="filter-input"
                        v-model="filter"
                        type="search"
                        placeholder="Type to Search"
                      ></b-form-input>

                      <b-input-group-append>
                        <b-button :disabled="!filter" @click="filter = ''"
                          >Clear</b-button
                        >
                      </b-input-group-append>
                    </b-input-group>
                  </b-form-group>
                </b-col>
                <b-col sm="5" md="6" class="my-1">
                  <b-form-group
                    label="Per page"
                    label-for="per-page-select"
                    label-cols-sm="6"
                    label-cols-md="4"
                    label-cols-lg="3"
                    label-align-sm="right"
                    label-size="sm"
                    class="mb-0"
                  >
                    <b-form-select
                      id="per-page-select"
                      v-model="perPage"
                      :options="pageOptions"
                      size="sm"
                    ></b-form-select>
                  </b-form-group>
                </b-col>
              </b-row>
              <b-table :items="buku" :fields="fields"
                :sort-by.sync="sortBy"
                striped
                responsive="sm"
                sort-icon-left
                :filter="filter"
                :filter-included-fields="filterOn"
                @filtered="onFiltered"
                :current-page="currentPage"
                :per-page="perPage">
                  <template slot="action" slot-scope="data">

                      <router-link
                        :to="{ name: 'edit-buku', params: { id: data.item.id } }"
                        class="btn btn-sm btn-primary"
                        >Edit</router-link
                      >
                      <button
                        class="btn btn-sm btn-danger"
                        @click="destroy(data.item.id)"
                      >
                        Hapus
                      </button>
                  </template>
              </b-table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios'
export default {
  data() {
    return {
      fields: [
        { key: "id", label: "Id" },
        { key: "judul", label: "Judul" },
        { key: "penulis", label: "Penulis" },
        { key: "penerbit", label: "Penerbit" },
        { key: "tanggal", label: "Tanggal" },
        { key: "kondisi", label: "Kondisi" },
        { key: "jumlah", label: "Jumlah" },
        { key: "lokasi.lokasi", label: "Lokasi" },
        { key: "action", label: "Action", sortable: false }
      ],
        filter: null,
        filterOn: [],
        currentPage: 1,
        perPage: 5,
        pageOptions: [5, 15, 25, 50,{ value: 100, text: "Show a lot" }],
      buku: [],
      sortBy: "id",
    };
  },
  created() {
    let uri = `/api/buku`;
    axios.get(uri).then((response) => {
      this.buku = response.data.buku;
    });
  },
  computed: {
    filterOn() {
      return this.fields
        .filter((field) => field.filterable)
        .map((field) => field.key);
    },
  },
  methods: {
    onFiltered(filteredItems) {
        this.totalRows = filteredItems.length
        this.currentPage = 1
    },
    destroy(id) {
      let uri = `/api/buku/${id}`;
      axios.delete(uri).then((response) => {
        this.buku = this.buku.filter((buku) => buku.id != id);
      });
    },
  },
};
</script>
