<template>
  <div class="home">
    <v-row>
      <v-col>
        <v-card elevation="2">
          <v-date-picker
            v-model="fecha"
            full-width
            locale="es"
            min="1984"
            :max="max"
            v-on:change="getDolar(fecha)"
          ></v-date-picker>
        </v-card>
        <v-card elevation="2" color="error" dark>
          <v-card-text class="display-1 text-center"> {{ valor }} </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      fecha: new Date().toISOString().substr(0, 10),
      max: new Date().toISOString().substr(0, 10),
      valor: null,
    };
  },
  methods: {
    async getDolar(dia) {
      let date = dia.split("-");
      dia = date.reverse().join("-");
      try {
        let datos = await axios.get(`https://mindicador.cl/api/dolar/${dia}`);
        // console.log(datos.data.serie[0].valor);
        this.valor =
          datos.data.serie.length > 0
            ? datos.data.serie[0].valor
            : "Sin resultados";
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.getDolar(this.fecha);
  },
};
</script>
