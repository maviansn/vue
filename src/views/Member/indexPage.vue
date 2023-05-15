<template>
  <div
    class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
  >
    <h1 class="h2">Dashboard Kasir</h1>
  </div>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <router-link
              :to="{ name: 'member.create' }"
              class="btn btn-md btn-success mb-5"
              >Tambah Member</router-link
            >
            <table class="table table-striped table-bordered mt- 4">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">Id</th>
                  <th scope="col">Nama</th>
                  <th scope="col">Alamat</th>
                  <th scope="col">Nomor Telepon</th>
                  <th scope="col">Tanggal Lahir</th>
                  <th scope="col">Email</th>
                  <th scope="col">Saldo Deposit</th>
                  <th scope="col">Masa Berlaku</th>
                  <th scope="col">Button</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(member, id) in members" :key="id">
                  <td>{{ member.id }}</td>
                  <td>{{ member.nama_member }}</td>
                  <td>{{ member.alamat_member }}</td>
                  <td>{{ member.telepon_member }}</td>
                  <td>{{ member.tanggal_lahir }}</td>
                  <td>{{ member.email }}</td>
                  <td>{{ member.sisa_deposit }}</td>
                  <td>{{ member.masa_aktivasi }}</td>
                  <td class="text-center">
                    <router-link
                      :to="{
                        name: 'member.edit',
                        params: { id: member.id },
                      }"
                      class="btn btn-success"
                    >
                      EDIT</router-link
                    >
                    <div>
                      <button
                        @click.prevent="memberDelete(member.id)"
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
    let members = ref([]);
    //mounted
    onMounted(() => {
      //get API from Laravel Backend
      axios
        .get("http://localhost:8000/api/member")
        .then((response) => {
          //assign state posts with response data
          members.value = response.data.data;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });

    function memberDelete(id) {
      axios
        .delete(`http://localhost:8000/api/member/${id}`)
        .then(() => {
          members.value.splice(members.value.indexOf(id), 1);
          location.reload(true);
        })
        .catch((error) => {
          console.log(error);
        });
    }
    //return
    return {
      members,
      memberDelete,
    };
  },
};
</script>
<style></style>
