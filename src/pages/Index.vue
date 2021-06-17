<template>
  <q-page padding>
  <q-table
      title="Data Kegiatan"
      :data="data"
      :columns="columns"
      row-key="namaKegiatan"
    >
      <template v-slot:top-right>
        <q-btn
          icon="add_box"
          label="Input Kegiatan"
          unelevated
          color="primary"
          :to="{ name:'inputKegiatan' }"
        />
      </template>
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="namaKegiatan" :props="props">
            {{ props.row.namaKegiatan }}
          </q-td>
          <q-td key="deskripsi" :props="props">
            {{ props.row.Deskripsi }}
          </q-td>
          <q-td key="waktu" :props="props">
            {{ props.row.Waktu }}
          </q-td>
          <q-td key="aksi" :props="props">
            <q-btn
              label = "Edit"
              icon = "edit"
              :to="{name: 'editKegiatan',params: { id: props.row._id }}"
              color = "warning"
              unelevated
            />
            <q-btn
              label = "Hapus"
              icon = "delete"
              color = "negative"
              @click="confirm(props.row._id)"
              class = "q-ml-md"
              unelevated
            />
          </q-td>
        </q-tr>
      </template>
    </q-table>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      columns: [
        {
           name: 'namaKegiatan',
           align: 'left',
           label: 'nama Kegiatan',
           field: 'namaKegiatan'
        },
        {
           name: 'deskripsi',
           align: 'left',
           label: 'Deskripsi',
           field: 'deskripsi'
        },
        {
           name: 'waktu',
           align: 'left',
           label: 'Waktu',
           field: 'waktu'
        },
        {
           name: 'aksi',
           align: 'left',
           label: 'Aksi',
           field: 'aksi'
        }
      ],
      data: [],
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('kegiatan/getall')
        .then(res =>{
          this.data = res.data.result
        })
    },
    confirm (id) {
      this.$q.dialog({
        title: 'konfirmasi',
        message: 'Apakah Anda Yakin?',
        cancel: true,
        persistent: true
      }).onOk(() => {
      this.$axios.delete(`kegiatan/delete/${id}`)
      .then(res =>{
        if (res.data.status){
          this.$q.notify({
            message: res.data.pesan,
            color: 'positive'
          })
          this.getData()
        }else{
          this.$q.notify({
            message: res.data.pesan,
            color: 'negative'
          })
        }
      })
      })
    }
  }
}
</script>
