<template>
  <div id="app">
    <h1>{{ msg }}</h1>

    <ul>
      <li><button name="entrada" v-on:click="verEntradas">Ver Entradas</button></li>
      <li><button name="pelicula" v-on:click="verPeliculas">Ver Peliculas</button></li>
    </ul>

    <div>
      <maestrodetalleEntrada v-show="mdEntradasVisible"></maestrodetalleEntrada>
    </div>
    <div>
      <maestrodetallePelicula v-show="mdPeliculasVisible"></maestrodetallePelicula>
    </div>

    
  </div>
</template>

<script>


import maestrodetalleEntrada from './components/MaestroDetalleEntrada.vue'
import maestrodetallePelicula from './components/MaestroDetallePelicula.vue'

export default {
  name: 'app',
  data () {
    return {
      mdPeliculasVisible : false,
      mdEntradasVisible: false,
      msg: 'Bienvenido al Maestro - Detalle',
      datos: {
        //disable: true;
        //entradas: [],
        //peliculas: [],
        //entrada: { Precio: "", Sala: "", Fila: "", Butaca: "", FechaEmision: "", Pelicula: "" },
        //pelicula: { Nombre: "", Director: "", Categoria: "", FechaEstreno: "", Pais: "", Duracion: ""}

      }
    }
  },
  components: {
    maestrodetalleEntrada, maestrodetallePelicula
  },

  methods: {

      verEntradas: function()
      {
        if(this.mdEntradasVisible == false)
        {
          this.mdEntradasVisible = true;
          this.mdPeliculasVisible = false;
        }
        else if(this.mdEntradasVisible == true)
        {
          this.mdEntradasVisible = false;
          this.mdPeliculasVisible = false;
        }
        
      },

      verPeliculas: function()
      {
        if(this.mdPeliculasVisible == false)
        {
          this.mdPeliculasVisible = true;
          this.mdEntradasVisible = false;
        }
        else if(this.mdPeliculasVisible == true)
        {
          this.mdPeliculasVisible = false;
          this.mdEntradasVisible = false;
        }
      }
        
    }
}
</script>


<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
