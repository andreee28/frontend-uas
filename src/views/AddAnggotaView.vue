<template>
    <div class="container pt-2">
        <h2>Tambah Anggota</h2>
    
        <table class="table table-responsive">






        
            <tbody>
                <tr>
                    <td>Nomor</td>
                    <td><input type="text" v-model="anggota.nomor" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Nama</td>
                    <td><input type="text" v-model="anggota.nama" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Jenis Kelamin</td>
                    <td><input type="text" v-model="anggota.jenis_kelamin" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Alamat</td>
                    <td><input type="text" v-model="anggota.alamat" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>No Hp</td>
                    <td><input type="text" v-model="anggota.no_hp" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tanggal Terdaftar</td>
                    <td><input type="date" v-model="anggota.tanggal_terdaftar" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td></td>
                    <td><button class="btn btn-success" @click="tambahAnggota">Tambah</button></td>
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
      anggota: {
        nomor : '',
        nama: '',
        jenis_kelamin: '',
        alamat: '',
        no_hp: '',
        tanggal_terdaftar: '',
      }
    };
  },
  created() {
    
  },
  methods: {
    tambahAnggota() {
        let formData = new FormData();
        formData.append('nomor', this.anggota.nomor)
        formData.append('nama', this.anggota.nama)
        formData.append('jenis_kelamin', this.anggota.jenis_kelamin)
        formData.append('alamat', this.anggota.alamat)
        formData.append('no_hp', this.anggota.no_hp)
        formData.append('tanggal_terdaftar', this.anggota.tanggal_terdaftar)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        console.log(this.anggota)
        axios({
            method: 'post',
            url: api + 'insertAnggota.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        // jika berhasil
            console.log('Anggota sukses ditambahkan:', response.data);
            this.$router.push('/anggota');
        })
        .catch(error => {
        // kalau gagal
        console.error('error:', error);
        });
    }
  }
}
</script>