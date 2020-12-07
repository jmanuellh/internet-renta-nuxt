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
          span(v-if="typeof nuevaRenta.id == 'undefined'" )
            v-btn(@click="agregarInternetRenta()" color="primary") Agregar renta
          span(v-else)
            v-btn(@click="actualizarInternetRenta()" color="warning") Actualizar renta
          
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
        template(v-slot:item.acciones="{ item }")
          v-btn( @click="actualizandoInternetRenta(item)" ) Editar
            

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
      },
      {
        text: "Acciones",
        value: "acciones"
      }
    ]
    }
  },
  mounted() {
    this.obtenerInternetRentas()
  },
  methods: {
    limpiarNuevaRenta() {
      this.nuevaRenta = {}
    },
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
    },
    actualizandoInternetRenta(item) {
      this.nuevaRenta = item
      this.nuevaRenta.fechaCorte = this.$moment(this.nuevaRenta.fechaCorte).format("YYYY-MM-DD")
    },
    async actualizarInternetRenta() {
      await this.$axios.$put("/internetRentas/"+this.nuevaRenta.id, this.nuevaRenta)
      this.limpiarNuevaRenta()
    }
  }
}
</script>
