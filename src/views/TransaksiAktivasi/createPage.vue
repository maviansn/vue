<template>
    <main>
      <div class="container mt-5">
        <div class="row">
          <div class="col-md-12">
            <div class="card border-0 rounded shadow">
              <div class="card-body">
                <h4>Transaksi Aktivasi Member</h4>
                <hr />
                <form @submit.prevent="store">
                  <div class="form-group mb-3">
                    <label class="form-label">Pegawai</label>
                    <select class="form-control" v-model="aktiv.id_pegawai">
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
                    <select class="form-control" v-model="aktiv.id_member">
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
                  <button type="submit" class="btn btn-primary">SIMPAN</button>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <div
      width="600px"
      id="printtarget"
      style="display: none; margin: 500px"
      class="text-dark"
    >
      <div width="600px" class="p-1">
        <table class="border border-dark">
          <tr>
            <td style="width: 70%">
              <strong>Gofit</strong>
              <p>Jl Centralpark No 10 Yogyakarta</p>
            </td>
            <td>No Struk : {{ aktiv.no_struk }}</td>
          </tr>
          <tr>
            <td></td>
            <td>Tanggal : {{ aktiv.tanggal_aktivasi }}</td>
          </tr>
          <tr></tr>
          <tr>
            <td>
              <table>
                <tr style="width: 80%">
                  <td><strong>Member</strong></td>
                  <td>:</td>
                  <td>{{ members.id_member }}</td>
                </tr>
                <tr>
                  <td>Aktivasi Tahunan</td>
                  <td>:</td>
                  <td>Rp.3.000.0000,-</td>
                </tr>
                <tr>
                  <td>Masa Aktif Member</td>
                  <td>:</td>
                  <td>{{ members.masa_berlaku_member }}</td>
                </tr>
              </table>
            </td>
          </tr>
          <tr>
            <td></td>
            <td>Kasir :{{ id_pegawai }}/ {{ nama_pegawai }}</td>
          </tr>
        </table>
      </div>
    </div>
  </template>
  <script>
  import { onMounted, reactive, ref } from "vue";
  import { useRouter } from "vue-router";
  // import { jsPDF } from "jspdf";
  import axios from "axios";
  export default {
    setup() {
      //state departemen
      const aktiv = reactive({
        id_member: "",
        id_pegawai: "",
      });
      //state validation
      const validation = ref([]);
      //vue router
      const router = useRouter();
      const members = ref([]);
      const pegawais = ref([]);
  
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
        let id_member = aktiv.id_member;
        let id_pegawai = aktiv.id_pegawai;
        axios
          .post("http://localhost:8000/api/transaksiAktivasi", {
            id_member: id_member,
            id_pegawai: id_pegawai,
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
      // function cetakStrukAktivasi() {
      //   console.log("cetak struk");
      //   let elementPrint = document.querySelector("#printtarget");
      //   elementPrint.style.display = "block";
      //   elementPrint.style.fontSize = "5px";
  
      //   //Spasi
      //   elementPrint.style.lineHeight = "1.2";
      //   elementPrint.style.margin = "0";
      //   elementPrint.style.padding = "0";
  
      //   let doc = new jsPDF({
      //     orientation: "l", // orientasi landscape
      //     unit: "mm", // satuan millimeter
      //     format: ["500", "200"], // ukuran kertas A4
      //   });
  
      //   doc.html(elementPrint, {
      //     callback: function (doc) {
      //       doc.save("struk.pdf");
      //       elementPrint.style.display = "none";
      //     },
      //     x: 10,
      //     y: 10,
      //   });
      //   console.log("akhir dari cetak pdf");
      // }
  
      //return
      return {
        aktiv,
        validation,
        router,
        store,
        // cetakStrukAktivasi,
        members,
        pegawais,
      };
    },
  };
  </script>
  <style></style>