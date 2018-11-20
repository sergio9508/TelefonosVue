<template>
  <v-container>
    <v-layout align-start justify-space-between row wrap>
      <v-flex xs4 sm2>
        <v-layout justify-center column>
          <h1>
            Clientes
          </h1>
          <v-combobox v-model="select" :items="items"></v-combobox>
          <v-btn color="primary" v-on:click="ObtenerClientes(select)">Buscar</v-btn>
        </v-layout>
      </v-flex>
      <v-flex xs4 sm4>
        <v-layout justify-center column>
          <h1>
            Informacion a mostrar
          </h1>
          <v-layout row wrap justify-start>
            <v-checkbox label="Nombre" v-model="checked1"></v-checkbox>
            <v-checkbox label="Genero" v-model="checked2"></v-checkbox>
            <v-checkbox label="Locacion" v-model="checked3"></v-checkbox>
            <v-checkbox label="Telefono" v-model="checked4"></v-checkbox>
            <v-checkbox label="Celular" v-model="checked5"></v-checkbox>
            <v-checkbox label="E-mail" v-model="checked6"></v-checkbox>
          </v-layout>
        </v-layout>
      </v-flex>
      <v-flex xs2 sm4>
        <v-layout justify-center column>
          <h1>
            Filtrar por nombre
          </h1>
          <v-text-field solo v-model="search" placeholder="Busqueda por nombre"></v-text-field>
        </v-layout>
      </v-flex>
    </v-layout>
    <v-layout align-center justify-space-around row fill-height />
    <div class="container">
      <div v-for="(cliente,conta) in searchUser" class="item">
        <v-img :src="cliente.picture.large" aspect-ratio="1" ></v-img>
        <h4 v-if="checked1">Nombre: {{cliente.name.first}}, {{cliente.name.last}} </h4>
        <h4 v-if="checked2">Sexo: {{cliente.gender}} </h4>
        <h4 v-if="checked3">Locacion: {{cliente.location.city}}</h4>
        <h4 v-if="checked4">Telefono: {{cliente.phone}}</h4>
        <h4 v-if="checked5">Celular: {{cliente.cell}}</h4>
        <h4 v-if="checked6">Correo: {{cliente.email}}</h4>
        <v-btn v-on:click="aceptarCliente(conta)">Aceptado</v-btn>
        <v-btn v-on:click="rechazarCliente(conta)">Rechazado</v-btn>
      </div>
      
    </div>
    <v-footer dark inset app color="indigo">
      <v-layout v-model="aceptado">
        <span>Aceptado: {{aceptado}} </span>
      </v-layout>
      <v-layout v-model="rechazado">
        <span> Rechazado: {{rechazado}}</span>
      </v-layout>
    </v-footer>
  </v-container>  

</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      bottomNav: 3,
      select: 5,
      items: [5, 10, 15],
      clientes: [],
      cantidad: 10,
      checked1: true,
      checked2: true,
      checked3: true,
      checked4: true,
      checked5: true,
      checked6: true,
      search: "",
      aceptado: 0,
      rechazado: 0
    };
  },
  created() {
    /*axios
        .get(`https://randomuser.me/api/?results=15&noinfo`)
        .then(response => {
          this.clientes = response.data.results;
        });*/
  },
  methods: {
    ObtenerClientes(number) {
      axios
        .get(`https://randomuser.me/api/?results=${number}&noinfo`)
        .then(response => {
          this.clientes = response.data.results;
        });
    },
    aceptarCliente(posicion) {
      //aceptado++;

      this.aceptado++;
      this.clientes.splice(posicion, 1);
    },

    rechazarCliente(posicion) {
      this.rechazado++;
      this.clientes.splice(posicion, 1);
    }
  },
  computed: {
    searchUser: function() {
      return this.clientes.filter(cliente => {
        return cliente.name.first.match(this.search);
      });
    }
  }
};
</script>

<style>
.container {
  min-height: 400px;
  display: flex;
  display: -webkit-flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  align-content: center;
}

.item {
  flex: 0 0 auto;
  margin: 10px;
}
</style>
