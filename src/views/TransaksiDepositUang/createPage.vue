<template>
  <main>
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-12">
          <div class="card border-0 rounded shadow">
            <div class="card-body">
              <h4>Transaksi Deposit Uang</h4>
              <hr />
              <form @submit.prevent="store">
                <div class="form-group mb-3">
                  <label class="form-label">Promo</label>
                  <select class="form-control" v-model="uang.id_promo">
                    <option hidden disabled selected>Pilih Promo</option>
                    <option
                      v-for="(promo, id) in promos"
                      :key="id"
                      :value="promo.id"
                    >
                      {{ promo.nama_promo }}
                    </option>
                  </select>
                  <!-- validation -->
                  <!-- <div v-if="validation.id_promo" class="mt-2 alert alert-danger">
                  {{ validation.id_promo[0] }}
                </div> -->
                </div>

                <div class="form-group mb-3">
                  <label class="form-label">Pegawai</label>
                  <select class="form-control" v-model="uang.id_pegawai">
                    <option hidden disabled selected>Pilih Pegawai</option>
                    <option
                      v-for="(pegawai, id) in pegawais"
                      :key="id"
                      :value="pegawai.id"
                    >
                      {{ pegawai.nama_pegawai }}
                    </option>
                  </select>
                  <!-- validation -->
                  <div
                    v-if="validation.id_pegawai"
                    class="mt-2 alert alert-danger"
                  >
                    {{ validation.id_pegawai[0] }}
                  </div>
                </div>

                <div class="form-group mb-3">
                  <label class="form-label">Member</label>
                  <select class="form-control" v-model="uang.id_member">
                    <option hidden disabled selected>Pilih Member</option>
                    <option
                      v-for="(member, id) in members"
                      :key="id"
                      :value="member.id"
                    >
                      {{ member.nama_member }}
                    </option>
                  </select>
                  <!-- validation -->
                  <div
                    v-if="validation.id_member"
                    class="mt-2 alert alert-danger"
                  >
                    {{ validation.id_member[0] }}
                  </div>
                </div>

                <div class="form-group mb-3">
                  <label class="form-label">Nominal</label>
                  <input
                    type="number"
                    class="form-control"
                    v-model="uang.nominal"
                    placeholder="Masukkan Jumlah Nominal Deposit"
                  />
                  <!-- validation -->
                  <div
                    v-if="validation.nominal"
                    class="mt-2 alert alert-danger"
                  >
                    {{ validation.nominal[0] }}
                  </div>
                </div>
                <button type="submit" class="btn btn-primary">SIMPAN</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<script>
import { onMounted, reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    //state departemen
    const uang = reactive({
      id_promo: "",
      id_member: "",
      id_pegawai: "",
      nominal: "",
    });
    //state validation
    const validation = ref([]);
    //vue router
    const router = useRouter();
    const members = ref([]);
    const pegawais = ref([]);
    const promos = ref([]);

    function getAllData() {
      axios
        .get("http://127.0.0.1:8000/api/member", {})
        .then((response) => {
          members.value = response.data.data;
          console.log(members.value);
        })
        .catch((error) => {
          console.log(error.response.data);
        });

      axios
        .get("http://127.0.0.1:8000/api/promoUang")
        .then((response) => {
          promos.value = response.data.data;
          console.log(promos.value);
        })
        .catch((error) => {
          console.log(error.response.data);
        });

      axios
        .get("http://127.0.0.1:8000/api/pegawai", {})
        .then((response) => {
          pegawais.value = response.data.data;
          console.log(pegawais.value);
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    }

    onMounted(() => {
      getAllData();
    });

    //method store
    function store() {
      let id_promo = uang.id_promo;
      let id_pegawai = uang.id_pegawai;
      let id_member = uang.id_member;
      let jumlah_deposit = uang.nominal;
      axios
        .post("http://localhost:8000/api/transaksiDepositUang", {
          id_promo: id_promo,
          id_pegawai: id_pegawai,
          id_member: id_member,
          jumlah_deposit: jumlah_deposit,
        })
        .then(() => {
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
      uang,
      validation,
      router,
      store,
      members,
      pegawais,
      promos,
    };
  },
};
</script>
<style></style>