<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>Update Member</h4>
            <hr />
            <form @submit.prevent="update">
              <div class="form-group mb-3">
                <label class="form-label">Nama Member</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="member.nama_member"
                  placeholder="Masukkan nama member"
                />
                <!-- validation -->
                <div
                  v-if="validation.nama_member"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.nama_member[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Alamat Member</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="member.alamat_member"
                  placeholder="Masukkan alamat member"
                />
                <!-- validation -->
                <div
                  v-if="validation.alamat_member"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.alamat_member[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Tanggal Lahir Member</label>
                <input
                  type="date"
                  class="form-control"
                  datepicker
                  v-model="member.tanggal_lahir"
                  placeholder="Masukkan tanggal lahir member"
                />
                <!-- validation -->
                <div
                  v-if="validation.tanggal_lahir"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.tanggal_lahir[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Email Member</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="member.email"
                  placeholder="Masukkan email member"
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
                  >Nomor Telepon Member</label
                >
                <input
                  class="form-control"
                  type="number"
                  v-model="member.telepon_member"
                  placeholder="Masukkan nomor telephone"
                />
                <!-- validation -->
                <div
                  v-if="validation.telepon_member"
                  class="mt-2 alert alert-danger"
                >
                  {{ validation.telepon_member[0] }}
                </div>
              </div>

              <div class="form-group mb-3">
                <label class="form-label">Password Member</label>
                <input
                  type="password"
                  class="form-control"
                  v-model="member.password"
                  placeholder="Masukkan password member"
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
    const member = reactive({
      nama_member: "",
      alamat_member: "",
      tanggal_lahir: "",
      email: "",
      telepon_member: "",
      password: "",
    });

    //state validation
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute();

    onMounted(() => {
      axios
        .get(`http://localhost:8000/api/member/${route.params.id}`)
        .then((response) => {
          member.nama_member = response.data.data.nama_member;
          member.alamat_member = response.data.data.alamat_member;
          member.tanggal_lahir = response.data.data.tanggal_lahir;
          member.email = response.data.data.email;
          member.telepon_member = response.data.data.telepon_member;
          member.password = response.data.data.password;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });

    //method update
    function update() {
      let nama_member = member.nama_member;
      let alamat_member = member.alamat_member;
      let telepon_member = member.telepon_member;
      let tanggal_lahir = member.tanggal_lahir;
      let email = member.email;
      let password = member.password;
      axios
        .put(`http://localhost:8000/api/member/${route.params.id}`, {
          nama_member: nama_member,
          alamat_member: alamat_member,
          telepon_member: telepon_member,
          tanggal_lahir: tanggal_lahir,
          email: email,
          password: password,
        })
        .then(() => {
          //redirect ke post index
          router.push({
            name: "member.index",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.data;
        });
    }
    //return
    return {
      member,
      validation,
      router,
      update,
      route,
    };
  },
};
</script>
<style></style>
