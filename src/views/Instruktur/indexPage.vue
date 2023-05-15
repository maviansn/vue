<template>
  <div
    class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
  >
    <h1 class="h2">Dashboard Admin</h1>
  </div>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <router-link
              :to="{ name: 'instruktur.create' }"
              class="btn btn-md btn-success mb-5"
              >Tambah Instruktur</router-link
            >
            <table class="table table-striped table-bordered mt- 4">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">Nama</th>
                  <th scope="col">Alamat</th>
                  <th scope="col">Nomor Telepon</th>
                  <th scope="col">Email</th>
                  <th scope="col">Button</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(instruktur, id) in instrukturs" :key="id">
                  <td>{{ instruktur.nama_instruktur }}</td>
                  <td>{{ instruktur.alamat_instruktur }}</td>
                  <td>{{ instruktur.telepon_instruktur }}</td>
                  <td>{{ instruktur.email }}</td>
                  <td class="text-center">
                    <router-link
                      :to="{
                        name: 'instruktur.edit',
                        params: { id: instruktur.id },
                      }"
                      class="btn btn-success"
                    >
                      EDIT</router-link
                    >
                    <div>
                      <button
                        @click.prevent="instrukturDelete(instruktur.id)"
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
    let instrukturs = ref([]);
    //mounted
    onMounted(() => {
      //get API from Laravel Backend
      axios
        .get("http://localhost:8000/api/instruktur")
        .then((response) => {
          //assign state posts with response data
          instrukturs.value = response.data.data;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });

    function instrukturDelete(id) {
      axios
        .delete(`http://localhost:8000/api/instruktur/${id}`)
        .then(() => {
          instrukturs.value.splice(instrukturs.value.indexOf(id), 1);
          location.reload(true);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    //return
    return {
      instrukturs,
      instrukturDelete,
    };
  },
};
</script>
<style></style>
