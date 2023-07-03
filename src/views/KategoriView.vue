<template>
  <div class="container pt-2">
 
      <a href="/addkategori" class="btn btn-primary my-2">Tambah</a>
      <table class="table">
          <thead>
              <tr>
              <th scope="col">ID</th>
              <th scope="col">Kode</th>
              <th scope="col">Kategori</th>
              <th scope="col">Aksi</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="k in kategori" :key="k.id">
                  <td>{{ k.id }}</td>
                  <td>{{ k.kode }}</td>
                  <td>{{ k.kategori }}</td>
                  <td class="d-flex justify-content-start">
                      <a :href="'/kategori/' + k.kode" class="btn btn-primary me-2">View</a>
                      <a class="btn btn-danger" @click="hapusKategori(k.kode)">Delete</a>
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
    kategori: [],
    errors: []
  }
},
created() {
  this.getData()
},
methods : {
  getData(){
    axios.get( api + 'selectKategori.php')
    .then(response => {
      // JSON responses are automatically parsed.
      this.kategori = response.data
      console.log(this.kategori)
    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  hapusKategori(kode) {
    // Menghapus buku berdasarkan kode
      console.log(kode)
      let formData = new FormData();
      formData.append('kode', kode)
      var bk = {};
      formData.forEach(function(value, key){
          bk[key] = value;
      });
      axios({
              method: 'post',
              url: api + 'deleteKategoribyKode.php',
              data: formData,
              config: { headers: {'Content-Type': 'multipart/form-data' }}
          })
      .then(response => {
          console.log('Kategori sukses dihapus:', response.data);
          // Refresh daftar buku setelah penghapusan
          this.getData();
      })
      .catch(error => {
          console.error('error:', error);
      });
  }
}
}
</script>
