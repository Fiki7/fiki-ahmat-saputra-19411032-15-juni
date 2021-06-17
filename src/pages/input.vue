<template>
<div class="q-pa-md" style="max-width: 400px">
    <q-form
      @submit="onSubmit"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="form.namaKegiatan"
        label="Nama Kegiatan"
        :rules="[ val => val && val.length > 0 || 'Mohon Masukkan Kegiatan']"
      />
      <q-input
        filled
        v-model="form.Deskripsi"
        label="Deskipsi Kegiatan"
        :rules="[ val => val && val.length > 0 || 'Mohon Isi Deskripsi Kegiatan']"
      />
      <q-input
        filled
        v-model="form.Waktu"
        label="Waktu Kegiatan"
        :rules="[ val => val && val.length > 0 || 'Mohon Isi Waktu Kegiatan']"
      />

      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
      </div>
    </q-form>

  </div>
</template>

<script>
export default {
  data(){
    return{
      form:{
        namaKegiatan : null,
        Deskripsi : null,
        Waktu : null
      }
    }
  },
  methods: {
    onSubmit() {
      this.$axios.post('kegiatan/insert', this.form)
      .then(res => {
        if (res.data.status){
          this.$q.notify({
            message: res.data.pesan,
            color: 'positive'
          })
          this.$router.push({ name:'dashboard' })
        } else{
            this.$q.notify({
            message: res.data.pesan,
            color: 'negative'
          })
        }
      })
    }
  }
}
</script>