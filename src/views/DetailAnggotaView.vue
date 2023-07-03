<template>
    <div class="container pt-2">
        <h2>Detail Anggota</h2>
        <table class="table table-responsive">
            <tbody>
                <tr>
                    <td>ID</td>
                    <td><input type="text" v-model="anggota.id" id="" class="form-control" readonly></td>
                </tr>
                <tr>
                    <td>Nomor</td>
                    <td><input type="text" v-model="anggota.nomor" id="" class="form-control" readonly></td>
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
                    <td>
                      <button class="btn btn-success me-2" @click="updateAnggota">Update</button>
                      <button class="btn btn-success" @click="backToAnggota">Kembali</button>
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
      anggota: {},
    };
  },
  created() {
    if (this.$route.params.nomor) {
        this.getDetailAnggota();
    }
  },
  methods: {
    getDetailAnggota() {
      const kode = this.$route.params.nomor;
      axios.get(api + `selectAnggotabyNomor.php?nomor=${kode}`)
        .then(response => {
          this.anggota = response.data.data;
          console.log(this.anggota)
        })
        .catch(error => {
          console.error(error);
        });
    },
    updateAnggota() {
      let formData = new FormData();
        formData.append('id', this.anggota.id)
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
            url: api + 'updateAnggotabyNomor.php',
            data: formData,
            config: { headers: {'Content-Type': 'multipart/form-data' }}
        })
        .then(response => {
        // kalau berhasil
            console.log('Anggota sukses diupdate:', response.data);
            window.location.reload();
        })
        .catch(error => {
        // kalau gagal
        console.error('error:', error);
        });
    },
    backToAnggota(){
      this.$router.push('/anggota');
    }
  }
}
</script>