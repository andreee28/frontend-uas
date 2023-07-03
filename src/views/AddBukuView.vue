<template>
    <div class="container pt-2">
        <h2>Tambah Buku</h2>
        <table class="table table-responsive">




            <tbody>
                <tr>
                    <td>Kode</td>
                    <td><input type="text" v-model="buku.kode" name="kode" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Judul</td>
                    <td><input type="text" v-model="buku.judul" name="judul" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Kode Kategori</td>
                    <td><input type="text" v-model="buku.kode_kategori" name="kode_kategori" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Penerbit</td>
                    <td><input type="text" v-model="buku.penerbit" name="penerbit" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Pengarang</td>
                    <td><input type="text" v-model="buku.pengarang" name="pengarang" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tahun</td>
                    <td><input type="number" v-model="buku.tahun" name="tahun" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tanggal Input</td>
                    <td><input type="date" v-model="buku.tanggal_input" name="tanggal_input" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Harga</td>
                    <td><input type="number" v-model="buku.harga" name="harga" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Cover</td>
                    <td><input type="text" v-model="buku.file_cover" name="cover" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td></td>
                    <td><button class="btn btn-success" @click="tambahBuku">Tambah</button></td>
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
      buku: {
        kode : '',
        kode_kategori: '',
        judul: '',
        pengarang: '',
        penerbit: '',
        tahun: 0,
        tanggal_input: '',
        harga: 0,
        file_cover: '',
      }
    };
  },
  created() {
    
  },
  methods: {
    tambahBuku() {
        let formData = new FormData();
        formData.append('name', this.buku.name)
        formData.append('kode', this.buku.kode)
        formData.append('kode_kategori', this.buku.kode_kategori)
        formData.append('judul', this.buku.judul)
        formData.append('pengarang', this.buku.pengarang)
        formData.append('penerbit', this.buku.penerbit)
        formData.append('tahun', this.buku.tahun)
        formData.append('tanggal_input', this.buku.tanggal_input)
        formData.append('harga', this.buku.harga)
        formData.append('file_cover', this.buku.file_cover)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        console.log(this.buku)
        axios({
            method: 'post',
            url: api + 'insertBuku.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
            console.log('Buku sukses ditambahkan:', response.data);
            this.$router.push('/buku');
        })
        .catch(error => {
    
        console.error('error:', error);
        });
    }
  }
}
</script>