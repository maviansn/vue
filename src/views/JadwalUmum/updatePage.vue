<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>Update Jadwal Umum</h4>
            <hr />
            <form @submit.prevent="store">

              <div class="form-group mb-3">
                <label class="form-label">Kelas</label>
                <select class="form-control" v-model="umum.id_kelas">
                  <option hidden disabled selected>Pilih Kelas</option>
                  <option v-for="(kelas, id) in kelass" :key="id">
                    {{ kelas.nama_kelas }}
                  </option>
                </select>
                <!-- validation -->
                <div v-if="validation.id_kelas" class="mt-2 alert alert-danger">
                  {{ validation.id_kelas[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Instruktur</label>
                <select class="form-control" v-model="umum.id_instruktur">
                  <option hidden disabled selected>Pilih Instruktur</option>
                  <option v-for="(instruktur, id) in instrukturs" :key="id">
                    {{ instruktur.nama_instruktur }}
                  </option>
                </select>
                <!-- validation -->
                <div
                  v-if="validation.id_instruktur"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.id_instruktur[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Jam Mulai</label>
                <input
                  type="time"
                  class="form-control"
                  timepicker
                  v-model="umum.waktu"
                  placeholder="Masukkan Jam Mulai"
                />
                <!-- validation -->
                <div
                  v-if="validation.waktu"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.waktu[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Hari</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="umum.hari"
                  placeholder="Masukkan Hari"
                />
                <!-- validation -->
                <div v-if="validation.hari" class="mt-2 alert alert-danger">
                  {{ validation.hari[0] }}
                </div>
              </div>

              <button type="submit" class="btn btn-primary" @click="update()">
                SIMPAN
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { onMounted, reactive, ref } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";
export default {
  setup() {
    //state departemen
    const umum = reactive({
      waktu: "",
      hari: "",
      id_kelas: "",
      id_instruktur: "",
    });
    //state validation
    const validation = ref([]);
    //vue router
    const router = useRouter();
    const instrukturs = ref([]);
    const kelass = ref([]);
    const route = useRoute();

    function getAllData() {
      axios
        .get("http://127.0.0.1:8000/api/instruktur", {})
        .then((response) => {
          instrukturs.value = response.data.data;
          console.log(instrukturs.value);
        })
        .catch((error) => {
          console.log(error.response.data);
        });

      axios
        .get("http://127.0.0.1:8000/api/kelas", {})
        .then((response) => {
          kelass.value = response.data.data;
          console.log(kelass.value);
        })
        .catch((error) => {
          console.log(error.response.data);
        });

      axios
        .get(`http://127.0.0.1:8000/api/jadwalUmum/${route.params.id}`, {})
        .then((response) => {
          umum.waktu = response.data.data.waktu;
          umum.hari = response.data.data.hari;
          umum.id_kelas = response.data.data.kelas;
          umum.id_instruktur = response.data.data.instruktur;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    }

    onMounted(() => {
      getAllData();
    });

    //method store
    function update() {
      let waktu = umum.waktu;
      let hari = umum.hari;
      let id_kelas = umum.id_kelas;
      let id_instruktur = umum.id_instruktur;
      axios
        .put(`http://localhost:8000/api/jadwalUmum/${route.params.id}`, {
          waktu: waktu,
          hari: hari,
          id_kelas: id_kelas,
          id_instruktur: id_instruktur,
        })
        .then(() => {
          //redirect ke post index
          router.push({
            name: "jadwalumum.index",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.data;
        });
    }
    //return
    return {
      umum,
      validation,
      router,
      update,
      kelass,
      instrukturs,
    };
  },
};
</script>
<style></style>
