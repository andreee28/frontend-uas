<template>
    <div class="container pt-2">
        <h2>Detail Buku</h2>
      
        <table class="table table-responsive">
            <tbody>
                <tr>
                    <td>ID</td>
                    <td><input type="text" v-model="buku.id" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Kode</td>
                    <td><input type="text" v-model="buku.kode" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Judul</td>
                    <td><input type="text" v-model="buku.judul" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Kode Kategori</td>
                    <td><input type="text" v-model="buku.kode_kategori" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Penerbit</td>
                    <td><input type="text" v-model="buku.penerbit" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Pengarang</td>
                    <td><input type="text" v-model="buku.pengarang" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tahun</td>
                    <td><input type="number" v-model="buku.tahun" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Harga</td>
                    <td><input type="number" v-model="buku.harga" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tanggal Input</td>
                    <td><input type="date" v-model="buku.tanggal_input" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td></td>
                    <td>
                      <button class="btn btn-success me-2" @click="updateBuku">Update</button>
                      <button class="btn btn-success" @click="backToBuku">Kembali</button>
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
      buku: {},
    };
  },
  created() {
    if (this.$route.params.kode) {
        this.getDetailBuku();
    }
  },
  methods: {
    getDetailBuku() {
      const kode = this.$route.params.kode;
      axios.get(api + `selectBukubyKode.php?kode=${kode}`)
        .then(response => {
          this.buku = response.data.data;
          console.log(this.buku)
        })
        .catch(error => {
          console.error(error);
        });
    },
    updateBuku() {
      let formData = new FormData();
        formData.append('id', this.buku.id)
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
            url: api + 'updateBukubyKode.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        //kalau berhasil
            console.log('Buku sukses diupdate:', response.data);
            window.location.reload();
        })
        .catch(error => {
        //jika salah
        console.error('error:', error);
        });
    },
    backToBuku(){
      this.$router.push('/buku');
    }
  }
}
</script>