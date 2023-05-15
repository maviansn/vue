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
              :to="{ name: 'jadwalumum.create' }"
              class="btn btn-md btn-success mb-5"
              >Tambah Jadwal Umum</router-link
            >

            <table class="table table-striped table-bordered mt- 4">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">Nama Kelas</th>
                  <th scope="col">Nama Instruktur</th>
                  <th scope="col">Hari</th>
                  <th scope="col">Jam Mulai</th>
                  <th scope="col">Button</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(umum, id) in umums" :key="id">
                  <td>{{ umum.id }}</td>
                  <td>{{ umum.kelas }}</td>
                  <td>{{ umum.instruktur }}</td>
                  <td>{{ umum.hari }}</td>
                  <td>{{ umum.waktu }}</td>
                  <td class="text-center">
                    <router-link
                      :to="{
                        name: 'jadwalumum.edit',
                        params: { id: umum.id },
                      }"
                      class="btn btn-success"
                    >
                      EDIT</router-link
                    >
                    <div>
                      <button
                        @click.prevent="umumDelete(umum.id)"
                        class="btn btn-danger ml-1"
                      >
                        DELETE
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
    let umums = ref([]);
    //mounted
    onMounted(() => {
      //get API from Laravel Backend
      axios
        .get("http://localhost:8000/api/jadwalUmum")
        .then((response) => {
          //assign state posts with response data
          umums.value = response.data.data;
        })
      .catch((error) => {
          console.log(error.response.data);
        });
    });

    function umumDelete(id) {
      axios
        .delete(`http://localhost:8000/api/jadwalUmum/${id}`)
        .then(() => {
          umums.value.splice(umums.value.indexOf(id), 1);
          location.reload(true);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    //return
    return {
      umums,
      umumDelete,
    };
  },
};
</script>
<style></style>
