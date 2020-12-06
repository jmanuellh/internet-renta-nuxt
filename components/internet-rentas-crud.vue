<template lang="pug">
  div
    div
      v-data-table(
        :items = "internetRentas"
        :headers = "cabeceras"
      )
        
</template>

<script>
export default {
  data() {
    return {
    internetRentas: [],
    cabeceras: [
      {
        text: "Nombre",
        value: 'nombre'
      },
      {
        text: "Fecha de corte",
        value: "fechaCorte"
      },
      {
        text: "Cantidad",
        value: "cantidad"
      }
    ]
    }
  },
  mounted() {
    this.obtenerInternetRentas()
  },
  methods: {
    async obtenerInternetRentas() {
      // this.internetRentas = await $axios.$get("")
      console.log(this.internetRentas)
    }
  }
}
</script>