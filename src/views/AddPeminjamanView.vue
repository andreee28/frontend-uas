<template>
    <div class="container pt-2">
        <h2>Tambah Peminjaman</h2>
    
        <table class="table table-responsive">








            <tbody>
                <tr>
                    <td>Anggota</td>
                    <td>
                        <select v-model="peminjaman.nomor_anggota" class="form-control">
                            <option v-for="a in anggota" :value="a.nomor">{{ a.nomor }}</option>
                        </select>  
                    </td>
                </tr>
                <tr>
                    <td>Judul</td>
                    <td>
                        <select v-model="peminjaman.kode_buku" class="form-control">
                            <option v-for="b in buku" :value="b.kode">{{ b.judul }}</option>
                        </select>  
                    </td>
                </tr>
                <tr>
                    <td>Tanggal Pinjam</td>
                    <td><input type="date" v-model="peminjaman.tanggal_peminjaman" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tanggal Kembali</td>
                    <td><input type="date" v-model="peminjaman.tanggal_pengembalian" class="form-control"></td>
                </tr>
                <tr>
                    <td></td>
                    <td><button class="btn btn-success" @click="tambahPeminjaman">Tambah</button></td>
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
      anggota: { },
      peminjaman : {},
      buku : {}
    };
  },
  created() {
    this.getAnggota();
    this.getBuku();
  },
  methods: {
    getAnggota(){
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
    getBuku(){
        axios.get( api + 'selectBuku.php')
        .then(response => {
        // JSON responses are automatically parsed.
            this.buku = [...response.data]
            console.log(this.buku)
        })
        .catch(e => {
            this.errors.push(e)
        })
    },
    tambahPeminjaman() {
        let formData = new FormData();
        formData.append('tanggal_peminjaman', this.peminjaman.tanggal_peminjaman)
        formData.append('tanggal_pengembalian', this.peminjaman.tanggal_pengembalian)
        formData.append('nomor_anggota', this.peminjaman.nomor_anggota)
        formData.append('kode_buku', this.peminjaman.kode_buku)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        console.log(this.buku)
        axios({
            method: 'post',
            url: api + 'insertPeminjaman.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        // kalau berhasil
            console.log('Peminjaman berhasil ditambahkan:', response.data);
            this.$router.push('/peminjaman');
        })
        .catch(error => {
        // jika salah/gagal
        console.error('error:', error);
        });
    }
  }
}
</script>