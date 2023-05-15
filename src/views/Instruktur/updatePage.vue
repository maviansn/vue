<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>Update Instruktur</h4>
            <hr />
            <form @submit.prevent="update">
              <div class="form-group mb-3">
                <label class="form-label">Nama Instruktur</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="instruktur.nama_instruktur"
                  placeholder="Masukkan nama instruktur"
                />
                <!-- validation -->
                <div
                  v-if="validation.nama_instruktur"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.nama_instruktur[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label for="content" class="form-label"
                  >Alamat Instruktur</label
                >
                <input
                  class="form-control"
                  type="text"
                  v-model="instruktur.alamat_instruktur"
                  placeholder="Alamat Instruktur"
                />
                <!-- validation -->
                <div
                  v-if="validation.alamat_instruktur"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.alamat_instruktur[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label for="content" class="form-label"
                  >Nomor Telepon Instruktur</label
                >
                <input
                  class="form-control"
                  type="number"
                  v-model="instruktur.telepon_instruktur"
                  placeholder="Masukkan nomor telepon"
                />
                <!-- validation -->
                <div
                  v-if="validation.telepon_instruktur"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.telepon_instruktur[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label for="content" class="form-label">Email Instruktur</label>
                <input
                  class="form-control"
                  v-model="instruktur.email"
                  placeholder="Masukkan email instruktur"
                />
                <!-- validation -->
                <div
                  v-if="validation.email"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.email[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label for="content" class="form-label"
                  >Password Instruktur</label
                >
                <input
                  class="form-control"
                  v-model="instruktur.password"
                  placeholder="Masukkan password instruktur"
                  type="password"
                />
                <!-- validation -->
                <div
                  v-if="validation.password"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.password[0] }}
                </div>
              </div>
              <button type="submit" class="btn btn-primary">SIMPAN</button>
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
    const instruktur = reactive({
      nama_instruktur: "",
      alamat_instruktur: "",
      telepon_instruktur: "",
      email: "",
      password: ""
    });
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute();

    onMounted(() => {
      axios
        .get(`http://localhost:8000/api/instruktur/${route.params.id}`)
        .then((response) => {
          instruktur.nama_instruktur = response.data.data.nama_instruktur;
          instruktur.alamat_instruktur = response.data.data.alamat_instruktur;
          instruktur.telepon_instruktur = response.data.data.telepon_instruktur;
          instruktur.email = response.data.data.email;
          instruktur.password = response.data.data.password;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });

    function update() {
      let nama_instruktur = instruktur.nama_instruktur;
      let alamat_instruktur = instruktur.alamat_instruktur;
      let telepon_instruktur = instruktur.telepon_instruktur;
      let email = instruktur.email;
      let password = instruktur.password;
      axios
        .put(`http://localhost:8000/api/instruktur/${route.params.id}`, {
          nama_instruktur: nama_instruktur,
          alamat_instruktur: alamat_instruktur,
          telepon_instruktur: telepon_instruktur,
          email: email,
          password: password
        })
        .then(() => {
          router.push({
            name: "instruktur.index",
          });
        })
        .catch((error) => {
          validation.value = error.response.data;
        });
    }
    return {
      instruktur,
      validation,
      router,
      update,
      //getData,
    };
  },
};
</script>
<style></style>
