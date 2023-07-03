<template>
    <div class="container pt-2">
        <h2>Detail Peminjaman</h2>
 
        <table class="table table-responsive">
            <tbody>
                <tr>
                    <td>ID</td>
                    <td><input type="text" v-model="peminjaman.id" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Anggota</td>
                    <td><input type="text" v-model="peminjaman.nama_anggota" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Judul</td>
                    <td><input type="text" v-model="peminjaman.judul_buku" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Tanggal Pinjam</td>
                    <td><input type="date" v-model="peminjaman.tanggal_peminjaman" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Tanggal Kembali</td>
                    <td><input type="date" v-model="peminjaman.tanggal_pengembalian" id="" class="form-control"></td>
                </tr>
                <tr>
                    <td>Status</td>
                    <td>
                        <select v-model="peminjaman.status_peminjaman" class="form-control">
                            <option value="DIPINJAM" :selected="peminjaman.status_peminjaman === 'DIPINJAM'">DIPINJAM</option>
                            <option value="DIKEMBALIKAN" :selected="peminjaman.status_peminjaman === 'DIKEMBALIKAN'">DIKEMBALIKAN</option>
                        </select>   
                    </td>
                </tr>
                <tr>
                    <td></td>
                    <td>
                      <button class="btn btn-success me-2" @click="updatePeminjaman">Update</button>
                      <button class="btn btn-success" @click="backToPeminjaman">Kembali</button>
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
      peminjaman: {
        id : ''
      },
    };
  },
  created() {
    if (this.$route.params.id) {
        this.getDetailpeminjaman();
    }
  },
  methods: {
    getDetailpeminjaman() {
      const id = this.$route.params.id;
      axios.get(api + `selectPeminjamanbyId.php?id=${id}`)
        .then(response => {
          this.peminjaman = response.data.data;
          console.log(response.data.data)
        })
        .catch(error => {
          console.error(error);
        });
    },
    updatePeminjaman() {
      let formData = new FormData();
        formData.append('id', this.peminjaman.id)
        formData.append('status_peminjaman', this.peminjaman.status_peminjaman)
        var bk = {};
        formData.forEach(function(value, key){
            bk[key] = value;
        });
        console.log(this.buku)
        axios({
            method: 'post',
            url: api + 'updatePeminjamanbyId.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        // kalau berhasil
            console.log('Peminjaman sukses diupdate:', response.data);
            window.location.reload();
        })
        .catch(error => {
        // kalo gagal
        console.error('error:', error);
        });
    },
    backToPeminjaman(){
      this.$router.push('/peminjaman');
    }
  }
}
</script>