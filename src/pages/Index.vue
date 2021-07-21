<template>
  <q-page padding>
<q-table
      title="Nama Warga"
      :rows="data"
      :columns="columns"
      row-key="name"
    >
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="namalengkap" :props="props">
            {{ props.row.namalengkap }}
          </q-td>
          <q-td key="nik" :props="props">
            {{ props.row.nik }}
          </q-td>
          <q-td key="ttl" :props="props">
            {{ props.row.ttl }}
          </q-td>
           <q-td key="alamat" :props="props">
            {{ props.row.alamat }}
          </q-td>
           <q-td key="jeniskelamin" :props="props">
            {{ props.row.jeniskelamin }}
          </q-td>
          <q-td key="agama" :props="props">
            {{ props.row.agama }}
          </q-td>
          <q-td key="aksi" :props="props">
            <div class="q-pa-md q-gutter-sm">
              <div class="col">
                <q-btn  label="Edit" :to="{ name: 'formEditWarga', params: { id: props.row._id}}" color="positive" icon="edit" unelevated />
               </div>
              <div class="col">
                <q-btn label="Hapus" @click="confirm(props.row._id)" color="negative" icon="delete" unelevated/>
               </div>
              </div>
          </q-td>
        </q-tr>
      </template>
    </q-table>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'namalengkap', align: 'center', label: 'Nama Lengkap', field: 'namalengkap', sortable: true },
        { name: 'nik', label: 'NIK', align: 'center', field: 'nik', sortable: true },
        { name: 'ttl', label: 'Tempat/Tgl/Lahir', align: 'center', field: 'ttl', sortable: true },
        { name: 'alamat', label: 'Alamat', align: 'center', field: 'alamat', sortable: true },
        { name: 'jeniskelamin', label: 'Jenis Kelamin', align: 'center', field: 'jeniskelamin', sortable: true },
        { name: 'agama', label: 'Agama', align: 'center', field: 'agama', sortable: true },
        { name: 'aksi', label: 'Aksi', field: 'aksi', align: 'center' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('warga/tampil')
        .then(res => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.$q.notify({
              type: 'negative',
              message: res.data.pesan
            })
          }
        })
    },
    confirm (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin Menghapus Data Warga Ini ?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete('warga/hapus/' + id)
          .then((res) => {
            if (res.data.sukses) {
              this.$q.notify({
                type: 'positive',
                message: res.data.pesan
              })
              this.getData()
            } else {
              this.$q.notify({
                type: 'negative',
                message: res.data.pesan
              })
            }
          })
      })
    }
  }
}
</script>
