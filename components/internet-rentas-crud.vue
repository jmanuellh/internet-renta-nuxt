<template lang="pug">
  div
    div
      v-form
        v-text-field(
          v-model="nuevaRenta.nombre"
          label = "Nombre"
        )
        v-text-field(
          v-model="nuevaRenta.fechaCorte"
          type="date"
          label = "Fecha de corte"
        )
        v-text-field(
          v-model.number="nuevaRenta.cantidad"
          type="number"
          label = "Cantidad"
        )
        div(class="d-flex justify-end" )
          v-btn(@click="agregarInternetRenta()" color="primary") Agregar renta
          
    div
      v-data-table(
        :items = "internetRentas"
        :headers = "cabeceras"
      )
        template(v-slot:item.cortado="{ item }")
          v-checkbox(
            v-model="item.cortado"
            @change="cambioEstadoCorte(item)"
          )
        template(v-slot:item.fechaCorte="{ item }")
          span {{ $moment(item.fechaCorte).format("DD/MM/YYYY") }}

</template>

<script>

export default {
  data() {
    return {
    nuevaRenta: {},
    internetRentas: [],
    cabeceras: [
      {
        text: "Cortado",
        value: "cortado"
      },
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
      this.internetRentas = await this.$axios.$get("/internetRentas")
      // this.internetRentas.forEach((element, index) => {
      //   // console.log('index: ', index)
      //   // element.fechaCorte
      // });
    },
    agregarInternetRenta() {
      this.$axios.post("/internetRentas", this.nuevaRenta).then(() => {
        this.obtenerInternetRentas()
      })
    },
    cambioEstadoCorte(item) {
      this.$axios.$put("/internetRentas/"+item.id, item)
    }
  }
}
</script>