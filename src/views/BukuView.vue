<template>
    <div class="container pt-2">
    
        <a href="/addbuku" class="btn btn-primary my-2">Input</a>
        <table class="table">
            <thead>
            <tr>
         <th scope="col">ID</th>
             <th scope="col">Judul</th>
             <th scope="col">Penerbit</th>
         <th scope="col">Pengarang</th>
             <th scope="col">Tahun</th>
           <th scope="col">Aksi</th>
             </tr>
            </thead>
            <tbody>
                <tr v-for="b in buku" :key="b.id">
                    <td>{{ b.id }}</td>
                    <td>{{ b.judul }}</td>
                    <td>{{ b.penerbit }}</td>
                    <td>{{ b.pengarang }}</td>
                    <td>{{ b.tahun }}</td>
                    <td class="d-flex justify-content-start">
                        <a :href="'/buku/' + b.kode" class="btn btn-primary me-2">lihat</a>
                        <a class="btn btn-danger" @click="hapusBuku(b.kode)">Hapus</a>
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
      buku: [],
      errors: []
    }
  },
  created() {
    this.getData()
  },
  methods : {
    getData(){
      axios.get( api + 'selectBuku.php')
      .then(response => {
    
        this.buku = response.data
        console.log(this.buku)
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    hapusBuku(kode) {
      // hapus yang berdasarkan kode
        console.log(kode)
        let formData = new FormData();
        formData.append('kode', kode)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        axios({
                method: 'post',
                url: api + 'deleteBukubyKode.php',
                data: formData,
                config: { headers: {'Content-Type': 'multipart/form-data' }}
            })
        .then(response => {
            console.log('sukses:', response.data);
            // Refresh daftar buku setelah penghapusan
            this.getData()
        })
        .catch(error => {
            console.error('error:', error);
        });
    }
  }
}
</script>
  