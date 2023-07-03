<template>
    <div class="container pt-2">
        <h2>Detail Kategori</h2>
     
        <table class="table table-responsive">
            <tbody>
                <tr>
                    <td>ID</td>
                    <td><input type="text" v-model="kategori.id" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Kode</td>
                    <td><input type="text" v-model="kategori.kode" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Kategori</td>
                    <td><input type="text" v-model="kategori.kategori" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td></td>
                    <td>
                      <button class="btn btn-success me-2" @click="updateKategori">Update</button>
                      <button class="btn btn-success" @click="backToKategori">Kembali</button>
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
      kategori: {},
    };
  },
  created() {
    if (this.$route.params.kode) {
        this.getDetailkategori();
    }
  },
  methods: {
    getDetailkategori() {
      const kode = this.$route.params.kode;
      axios.get(api + `selectKategoribyKode.php?kode=${kode}`)
        .then(response => {
          this.kategori = response.data.data;
          console.log(this.buku)
        })
        .catch(error => {
          console.error(error);
        });
    },
    updateKategori() {
      let formData = new FormData();
        formData.append('id', this.kategori.id)
        formData.append('kode', this.kategori.kode)
        formData.append('kategori', this.kategori.kategori)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        console.log(this.buku)
        axios({
            method: 'post',
            url: api + 'updateKategoribyKode.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        // kalau berhasil
            console.log('Kategori sukses diupdate:', response.data);
            window.location.reload();
        })
        .catch(error => {
        // kalo gagal
        console.error('error:', error);
        });
    },
    backToKategori(){
      this.$router.push('/kategori');
    }
  }
}
</script>