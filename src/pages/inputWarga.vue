<template>
    <q-page padding>
    <q-card flat class="q-gutter-md">
      <q-card-section>
          <div class="text-h5">Input Warga</div>
          <q-form
               @submit="onSubmit"
               @reset="onReset"
               class="q-mt-lg"
          >
          <div class="q-gutter-md">
              <q-input label="Nama Lengkap" v-model="namalengkap" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Nama Anda']"></q-input>
              <q-input label="NIK" v-model="nik" filled :rules="[ val => val && val.length > 0 || 'Tolong isi NIK Anda']"></q-input>
              <q-input label="Tempat/Tgl/Lahir" v-model="ttl" filled :rules="[ val => val && val.length > 0 || 'Tolong isi ttl']"></q-input>
              <q-input label="Alamat" v-model="alamat" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Alamat Lengkap']"></q-input>
              <q-input label="Jenis Kelamin" v-model="jeniskelamin" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Jenis Kelamin']"></q-input>
              <q-input label="Agama" v-model="agama" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Agama']"></q-input>
              <q-btn type="submit" label="Input" color="positive" unelevated></q-btn>
              <q-btn type="reset" label="Reset" color="positive" flat unelevated></q-btn>
          </div>
          </q-form>
      </q-card-section>
    </q-card>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      namalengkap: null,
      nik: null,
      ttl: null,
      alamat: null,
      jeniskelamin: null,
      agama: null
    }
  },
  methods: {
    onReset () {
      this.namalengkap = null
      this.nik = null
      this.ttl = null
      this.alamat = null
      this.jeniskelamin = null
      this.agama = null
    },
    onSubmit () {
      this.$axios.post('warga/insert', {
        namalengkap: this.namalengkap,
        nik: this.nik,
        ttl: this.ttl,
        alamat: this.alamat,
        jeniskelamin: this.jeniskelamin,
        agama: this.agama
      }).then(res => {
        if (res.data.sukses) {
          this.$q.notify({
            type: 'positive',
            message: res.data.pesan
          })
          this.$router.push({ name: 'dashboard' })
        } else {
          this.$q.notify({
            type: 'negative',
            message: res.data.pesan
          })
        }
      })
    }
  }
}
</script>
