<template>
    <div id="maestrodetalleEntrada">

        <div name="listaEntradas">
            <ul>
                <li>
                    <h1>Entradas</h1>
                </li>
                <li v-for="entrada in entradas">
                    <label for="nombreP" v-on:click="mostrarDetalles(entrada.Id)"> Pelicula: </label>
                    <input type="text" name="nombreP" v-model="entrada.Pelicula" disabled="true">
                    <label for="precio"> Precio: </label>
                    <input type="text" name="precio" value="Precio" v-model="entrada.Precio" disabled="true">
                    
                </li> 
            </ul>

            <br>
            <button v-on:click="editable(modos.nuevo)">Nueva Entrada</button>
            <br>

        </div>

        <div name="detalles" v-show="mostrarDetallesContenedor">
            <ul>
                <li>
                    <h1 v-show="modoDetalle"> Entrada : </h1>
                </li>
                <li>
                    <h1 v-show="modoNuevo"> Nueva Entrada : </h1>
                </li>
                <li>
                    <label for="nombreP"> Pelicula: </label>
                    <input type="text" name="nombreP" value="Pelicula" v-model="entrada.Pelicula" v-bind:disabled="disable">
                    
                    <label for="precio"> Precio: </label>
                    <input type="numeric" name="precio" value="Precio" v-model="entrada.Precio" v-bind:disabled="disable">
                   
                    <label for="sala"> Sala: </label>
                    <input type="numeric" name="sala" value="Sala" v-model="entrada.Sala" v-bind:disabled="disable">
                   
                    <label for="fila"> Fila: </label>
                    <input type="numeric" name="fila" value="Fila" v-model="entrada.Fila" v-bind:disabled="disable">
                    
                    <label for="butaca"> Butaca: </label>
                    <input type="numeric" name="butaca" value="Butaca" v-model="entrada.Butaca" v-bind:disabled="disable">

                    <button v-on:click="editable(modos.editar)" v-show="btnEditElim">Editar</button>
                    <button v-on:click="editable(modos.eliminar)" v-show="btnEditElim">Eliminar</button>
                    <button v-on:click="cerrarDetalles" v-show="btnEditElim">Cerrar</button>
                    
                    <br>
                    <br>
                    <button v-on:click="crearEntrada" v-show="btnAceptarCancelar">Aceptar</button>
                    <button v-on:click="cancelar" v-show="btnAceptarCancelar">Cancelar</button>

                    <br>
                    <button v-on:click="actualizar" v-show="btnACtCancelar">Actualizar</button>
                    <button v-on:click="cancelar" v-show="btnACtCancelar">Cancelar</button>

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
            btnAceptarCancelar: false,
            btnEditElim: false,
            btnACtCancelar: false,
            modoNuevo: false,
            modoDetalle: true,
            entrada: {Id: "", Precio: "", Sala: "", Fila: "", Butaca: "", Pelicula: "" },
            modos: {editar: "editar", eliminar: "eliminar", nuevo: "nuevo"}
        };
    },
    methods: {

        editable: function(mode)
        {
            if(this.disable == true && mode == "editar")
            {
                alert("estoy en editar");
                this.disable = false;
                this.btnAceptarCancelar = false;
                this.btnEditElim = false;
                this.btnACtCancelar = true;
               // this.actualizar();
            }
            else if(this.disable == true && mode == "nuevo")
            {
                this.modoNuevo = true;
                this.modoDetalle = false;
                this.mostrarDetallesContenedor = true;
                this.disable = false;
                this.btnEditElim = false;
                this.btnAceptarCancelar = true;
                this.limpiarCampos();
            }
            else if(this.disable == true && mode == "eliminar")
            {
                this.disable = false;
                this.eliminarEntrada();

            }
            else if(this.disable = false)
            {
                this.disable = true;
            }
        },

        crearEntrada: function()
        {
            var _this = this;

            $.ajax({
              type: "POST",
              url: "http://localhost:51673/api/Entradas",
              data: { Precio: _this.entrada.Precio, Sala: _this.entrada.Sala, Fila: _this.entrada.Fila, Butaca: _this.entrada.Butaca, 
                Pelicula: _this.entrada.Pelicula},

            })
            .done(function(data) {

              alert( "Creado la entrada -> " + "Id: " + data.Id + " Precio: " + data.Precio + " Sala: " + data.Sala + " Fila: " + data.Fila +
                "Butaca" + data.Butaca + "Pelicula" + data.Pelicula);

              _this.refreshList();
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;
              

          })
          .fail(function(data) {
              alert( "ERROR, No se ha podido crear la Entrada" );
          });

        },

        eliminarEntrada: function()
        {
             var _this = this;

            $.ajax({

              type: "DELETE",
              url: "http://localhost:51673/api/Entradas/" + _this.entrada.Id,
              data: { Id: _this.entrada.Id }

            })
            .done(function(data) {

              alert( "Eliminado la entrada -> " + "Id: " + data.Id + " Precio: " + data.Precio + " Sala: " + data.Sala + " Fila: " + data.Fila 
                + "Butaca: " + data.Butaca + "Pelicula: " + data.Pelicula);
              _this.refreshList();
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;

            })
            .fail(function(data) {
              alert( "ERROR, al eliminar la Entrada" );
          });

        },

        actualizar: function()
        {
            var _this = this;

            $.ajax({

            type: "PUT",
            url: "http://localhost:51673/api/Entradas/" + _this.entrada.Id,
            data: _this.entrada,

            })
            .done(function(data) {

                alert( "Se ha actualizado la Entrada");
                _this.refreshList();
                _this.limpiarCampos();
                _this.modoNuevo = false;
                _this.modoDetalle = true;
                _this.btnAceptarCancelar = false;
                _this.btnACtCancelar = false;
                _this.disable = true;
                _this.mostrarDetallesContenedor = false;
                
            })
            .fail(function(data) {
                alert( "ERROR, al actualizar la Entrada" );
            });

        },

        mostrarDetalles: function(id)
        {

                if (this.mostrarDetallesContenedor == false) 
                {
                    this.mostrarDetallesContenedor = true;
                    this.btnEditElim = true;
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

          refreshList: function()
          {

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
          },

        limpiarCampos: function()
        {
            this.entrada = {Id: "", Precio: "", Sala: "", Fila: "", Butaca: "", Pelicula: "" };
        },

        cancelar: function()
        {
            var _this = this;

            _this.refreshList();
            _this.limpiarCampos();
            _this.modoNuevo = false;
            _this.modoDetalle = true;
            _this.btnAceptarCancelar = false;
            _this.disable = true;
            _this.mostrarDetallesContenedor = false;
        },

        cerrarDetalles: function()
        {
            var _this = this;

            _this.refreshList();
            _this.limpiarCampos();
            _this.modoNuevo = false;
            _this.modoDetalle = true;
            _this.btnAceptarCancelar = false;
            _this.disable = true;
            _this.mostrarDetallesContenedor = false;
        }

    },

   mounted: function() {
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
