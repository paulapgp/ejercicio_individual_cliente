<template>
    <div id="maestrodetalleEntrada">

        
        <!--<button v-on:click="editable(modos.nuevo)">Nueva Entrada</button>-->

        <div name="listaEntradas">
            <ul>
                <li>
                    <h1>Entradas</h1>
                </li>
                <li v-for="entrada in entradas">
                    <label for="nombreP"> Pelicula: </label>
                    <input type="text" name="nombreP" v-model="entrada.Pelicula" v-bind:disabled="disable" v-select="mostrarDetalles(entrada.Id)">
                    <label for="precio"> Precio: </label>
                    <input type="text" name="precio" value="Precio" v-model="entrada.Precio" v-bind:disabled="disable">
                    
                </li> 
            </ul>
        </div>

        <div name="detalles" v-show="this.mostrarDetallesContenedor">
            <ul>
                <li>
                    <h1>Entrada:</h1>
                </li>
                <li>
                    <label for="nombreP" v-select=""> Pelicula: </label>
                    <input type="text" name="nombreP" value="Pelicula" v-model="entrada.Pelicula" v-bind:disabled="disable">
                    <label for="precio"> Precio: </label>
                    <input type="text" name="precio" value="Precio" v-model="entrada.Precio" v-bind:disabled="disable">
                    <button v-on:click="editable(modos.editar)">Editar</button>
                    <button v-on:click="editable(modos.eliminar)">Eliminar</button>
                </li> 
            </ul>
        </div>

    </div>
</template>

<script>

import $ from 'jquery'

export default {
    name: 'maestrodetalleEntrada',
    //props: ['datapadre'],
    data() {
        return {

            mostrarDetallesContenedor: false,
            disable: true,
            entradas: [],
            entrada: {Id: "", Precio: "", Sala: "", Fila: "", Butaca: "", Pelicula: "" },
            modos: {editar: "editar", eliminar: "eliminar", nuevo: "nuevo"}
        };
    },
    methods: {

        crearEntrada: function()
        {
            //this.editable();
        },

        editable: function(mode)
        {
            if(this.disable == true && mode == "editar")
            {
                alert("estoy en editar");
                this.disable = false;
                //llamar a update
            }
            else if(this.disable == true && mode == "nuevo")
            {
                alert("estoy en nuevo");
                this.disable = false;
                this.limpiarCampos();
            }
            else if(this.disable == true && mode == "eliminar")
            {
                alert("estoy en eliminar");
                this.disable = false;
            }
            else if(this.disable = false)
            {
                alert("pongo la variable a true para deshabilitar");
                this.disable = true;
            }
        },

        limpiarCampos: function()
        {
            this.entrada = { Precio: "", Sala: "", Fila: "", Butaca: "", Pelicula: "" };
        },

        mostrarDetalles: function(id)
          {

                if (this.mostrarDetallesContenedor == false) 
                {
                    this.mostrarDetallesContenedor = true;
                }

                var _this = this;
                $.ajax(
                {
                  url : "http://localhost:51673/api/Entradas/" + id,
                  type: "GET",
                })
                .done(function(data) {

                  _this.entrada.Id = data.Id;
                  _this.entrada.Precio = data.Precio;
                  _this.entrada.Sala = data.Sala;
                  _this.entrada.Fila = data.Fila;
                  _this.entrada.Butaca = data.Butaca;
                  _this.entrada.Pelicula = data.Pelicula;

                })
                .fail(function(data) {
                        alert( "error" );
                      });
          },


        
    },

   created: function() {
          var _this = this;
          $.ajax(
            {
              url : "http://localhost:51673/api/Entradas",
              type: "GET",
            })
            .done(function(data) {
              _this.entradas = data;
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
