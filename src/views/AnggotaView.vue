<template>
  <div class="container pt-2">
    
      <a href="/addanggota" class="btn btn-primary my-2">Tambah</a>
      <table class="table">
          <thead>
              <tr>
              <th scope="col">ID</th>
              <th scope="col">Nomor</th>
              <th scope="col">Nama</th>
              <th scope="col">Jenis Kelamin</th>
              <th scope="col">Alamat</th>
              <th scope="col">No Hp</th>
              <th scope="col">Tanggal Terdaftar</th>
              <th scope="col">Aksi</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="a in anggota" :key="a.id">
                  <td>{{ a.id }}</td>
                  <td>{{ a.nomor }}</td>
                  <td>{{ a.nama }}</td>
                  <td>{{ a.jenis_kelamin }}</td>
                  <td>{{ a.alamat }}</td>
                  <td>{{ a.no_hp }}</td>
                  <td>{{ a.tanggal_terdaftar }}</td>
                  <td class="d-flex justify-content-start">
                      <a :href="'/anggota/' + a.nomor" class="btn btn-primary me-2">View</a>
                      <a class="btn btn-danger" @click="hapusanggota(a.nomor)">Delete</a>
                  </td>
              </tr>
          </tbody>
      </table>
  </div>
</template>

<script>
import axios from 'axios';
import api from '../setting/api'

export default {
data() {
  return {
    anggota: [],
    errors: []
  }
},
created() {
  this.getData()
},
methods : {
  getData(){
    axios.get( api + 'selectAnggota.php')
    .then(response => {
      // JSON responses are automatically parsed.
      this.anggota = [...response.data]
      console.log(this.anggota)
    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  hapusanggota(nomor) {
    // Menghapus anggota berdasarkan kode
      console.log(nomor)
      let formData = new FormData();
      formData.append('nomor', nomor)
      var bk = {};
      formData.forEach(function(value, key){
          bk[key] = value;
      });
      axios({
              method: 'post',
              url: api + 'deleteAnggotabyNomor.php',
              data: formData,
              config: { headers: {'Content-Type': 'multipart/form-data' }}
          })
      .then(response => {
          console.log('anggota sukses dihapus:', response.data);
          // Refresh daftar anggota setelah penghapusan
          this.getData()
      })
      .catch(error => {
          console.error('error:', error);
      });
  }
}
}
</script>
