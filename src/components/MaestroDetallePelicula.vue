<template>
    <div id="maestrodetallePelicula">

        <h1>Peliculas</h1>
        <button v-on:click="crearPelicula">Nueva Pelicula</button>
        <ul>
            <li v-for="pelicula in peliculas">
               Pelicula: {{pelicula.Nombre}} || Director: {{pelicula.Director}} || Categoria: {{pelicula.Categoria}}
            </li>
        </ul>

    </div>
</template>


<script>

import $ from 'jquery'

export default {
    name: 'maestrodetallePelicula',
    //props: ['datapadre'],
    data() {
        return {

            peliculas: [],
            pelicula: { Nombre: "", Director: "", Categoria: "", Pais: "", Duracion: ""}
        };
    },
    methods: {

        crearPelicula: function()
        {
            alert("estoy en crearPelicula");
        }


    },

   created: function() {
          var _this = this;
          $.ajax(
            {
              url : "http://localhost:51673/api/Peliculas",
              type: "GET",
            })
            .done(function(data) {
              _this.peliculas = data;
            })
            .fail(function(data) {
                    alert( "error" );
                  });

        }
}

</script>

<style scoped>
#maestro {
    width: 50%;
}

h1{
    width:100%;
}

ul {
    width: 100%;
}

li {
    list-style-type: none;
    width:100%;
}

li:hover {
    cursor: pointer;
}

.selected{
    background-color:brown;
}
</style>
