<template>
    <div class="container pt-2">

        <a href="/addpeminjaman" class="btn btn-primary my-2">Tambah</a>
        <table class="table">
            <thead>
                <tr>
                <th scope="col">ID</th>
                <th scope="col">Judul</th>
                <th scope="col">Anggota</th>
                <th scope="col">Tanggal Pinjam</th>
                <th scope="col">Tanggal Kembali</th>
                <th scope="col">Status</th>
                <th scope="col">Aksi</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="p in peminjaman" :key="p.id">
                    <td>{{ p.id }}</td>
                    <td>{{ p.judul_buku }}</td>
                    <td>{{ p.nama_anggota }}</td>
                    <td>{{ p.tanggal_peminjaman }}</td>
                    <td>{{ p.tanggal_pengembalian }}</td>
                    <td>{{ p.status_peminjaman }}</td>
                    <td class="d-flex justify-content-start">
                        <a :href="'/peminjaman/' + p.id" class="btn btn-primary me-2">View</a>
                        <a class="btn btn-danger" @click="hapusPeminjaman(p.id)">Delete</a>
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
      peminjaman: [],
      errors: []
    }
  },
  created() {
    this.getData()
  },
  methods : {
    getData(){
      axios.get( api + 'selectPeminjaman.php')
      .then(response => {
        // JSON responses are automatically parsed.
        this.peminjaman = response.data
        console.log(this.peminjaman)
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    hapusPeminjaman(id) {
      // Menghapus buku berdasarkan kode
        console.log(id)
        let formData = new FormData();
        formData.append('id', id)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        axios({
                method: 'post',
                url: api + 'deletePeminjamanbyId.php',
                data: formData,
                config: { headers: {'Content-Type': 'multipart/form-data' }}
            })
        .then(response => {
            console.log('Peminjaman sukses dihapus:', response.data);
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
  