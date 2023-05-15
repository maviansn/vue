<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>Tambah Instruktur</h4>
            <hr />
            <form @submit.prevent="store">
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
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
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
    //state validation
    const validation = ref([]);
    //vue router
    const router = useRouter();
    //method store
    function store() {
      let nama_instruktur = instruktur.nama_instruktur;
      let alamat_instruktur = instruktur.alamat_instruktur;
      let telepon_instruktur = instruktur.telepon_instruktur;
      let email = instruktur.email;
      let password = instruktur.password;
      axios
        .post("http://localhost:8000/api/instruktur", {
          nama_instruktur: nama_instruktur,
          alamat_instruktur: alamat_instruktur,
          telepon_instruktur: telepon_instruktur,
          email: email,
          password: password
        })
        .then(() => {
          //redirect ke post index
          router.push({
            name: "instruktur.index",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.data;
        });
    }
    //return
    return {
      instruktur,
      validation,
      router,
      store,
    };
  },
};
</script>
<style></style>
