<template>
  <div
    class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
  >
    <h1 class="h2">Dashboard Manager Operasional</h1>
  </div>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <router-link
              :to="{ name: 'aktivasi.create' }"
              class="btn btn-md btn-success mb-5"
              >Tambah Transaksi Aktivasi</router-link
            >

            <table class="table table-striped table-bordered mt- 4">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">No Struk</th>
                  <th scope="col">Member</th>
                  <th scope="col">Pegawai</th>
                  <th scope="col">Jumlah Bayar</th>
                  <th scope="col">Tanggal Transaksi</th>
                  <th scope="col">Tanggal Expired</th>
                  <th scope="col">Button</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(aktivasi, id) in aktivasis" :key="id">
                  <td>{{ aktivasi.id }}</td>
                  <td>{{ aktivasi.member }}</td>
                  <td>{{ aktivasi.pegawai }}</td>
                  <td>{{ aktivasi.jumlahBayar }}</td>
                  <td>{{ aktivasi.tglTransaksi }}</td>
                  <td>{{ aktivasi.tglExpired }}</td>
                  <td class="text-center">
                    <div>
                      <button
                        @click.prevent="aktivasiDelete(aktivasi.id)"
                        class="btn btn-danger ml-1"
                      >
                        Cancel
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import { onMounted, ref } from "vue";
export default {
  setup() {
    //reactive state
    let aktivasis = ref([]);
    //mounted
    onMounted(() => {
      //get API from Laravel Backend
      axios
        .get("http://localhost:8000/api/transaksiAktivasi")
        .then((response) => {
          //assign state posts with response data
          aktivasis.value = response.data.data;
        })
      .catch((error) => {
          console.log(error.response.data);
        });
    });

    function aktivasiDelete(id) {
      axios
        .delete(`http://localhost:8000/api/transaksiAktivasi/${id}`)
        .then(() => {
          aktivasis.value.splice(aktivasis.value.indexOf(id), 1);
          location.reload(true);
        })
        .catch((error) => {
          console.log(error);
        });
    }

    //return
    return {
      aktivasis,
      aktivasiDelete
    };
  },
};
</script>
<style></style>
