<template>
    <div id="maestrodetallePelicula">

        <div name="listaPeliculas">
            <ul>
                <li>
                    <h1>Peliculas</h1>
                </li>
                <li v-for="pelicula in peliculas">
                    <label for="nombre" v-on:click="mostrarDetalles(pelicula.Id)"> Titulo: </label>
                    <input type="text" name="nombre" v-model="pelicula.Nombre" disabled="true">
                    <label for="categoria"> Categoria: </label>
                    <input type="text" name="categoria" v-model="pelicula.Categoria" disabled="true">
                    
                </li> 
            </ul>

            <br>
            <button v-on:click="editable(modos.nuevo)">Nueva Pelicula</button>
            <br>

        </div>

        <div name="detalles" v-show="mostrarDetallesContenedor">
            <ul>
                <li>
                    <h1 v-show="modoDetalle"> Pelicula : </h1>
                </li>
                <li>
                    <h1 v-show="modoNuevo"> Nueva Pelicula : </h1>
                </li>
                <li>
                    <label for="nombreP"> Titulo: </label>
                    <input type="text" name="nombreP" value="Nombre" v-model="pelicula.Nombre" v-bind:disabled="disable">
                    
                    <label for="director"> Director: </label>
                    <input type="text" name="director" value="Director" v-model="pelicula.Director" v-bind:disabled="disable">
                   
                    <label for="categoria"> Categoria: </label>
                    <input type="text" name="categoria" value="Categoria" v-model="pelicula.Categoria" v-bind:disabled="disable">
                   
                    <label for="duracion"> Duracion: </label>
                    <input type="numeric" name="duracion" value="Duracion" v-model="pelicula.Duracion" v-bind:disabled="disable">
                    
                    <label for="pais"> Pais: </label>
                    <input type="text" name="pais" value="Pais" v-model="pelicula.Pais" v-bind:disabled="disable">

                    <button v-on:click="editable(modos.editar)" v-show="btnEditElim">Editar</button>
                    <button v-on:click="editable(modos.eliminar)" v-show="btnEditElim">Eliminar</button>
                    <button v-on:click="cerrarDetalles" v-show="btnEditElim">Cerrar</button>
                    
                    <br>
                    <br>
                    <button v-on:click="crearPelicula" v-show="btnAceptarCancelar">Aceptar</button>
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
    name: 'maestrodetallePelicula',
    //props: ['datapadre'],
    data() {
        return {

            mostrarDetallesContenedor: false,
            disable: true,
            peliculas: [],
            btnAceptarCancelar: false,
            btnEditElim: false,
            btnACtCancelar: false,
            modoNuevo: false,
            modoDetalle: true,
            pelicula: {Id: "", Nombre: "", Director: "", Categoria: "", Duracion: "", Pais: "" },
            modos: {editar: "editar", eliminar: "eliminar", nuevo: "nuevo"}
        };
    },
    methods: {

        editable: function(mode)
        {
            if(this.disable == true && mode == "editar")
            {
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
                this.eliminarPelicula();

            }
            else if(this.disable = false)
            {
                this.disable = true;
            }
        },

        crearPelicula: function()
        {
            var _this = this;

            $.ajax({
              type: "POST",
              url: "http://localhost:51673/api/Peliculas",
              data: { Nombre: _this.pelicula.Nombre, Director: _this.pelicula.Director, Categoria: _this.pelicula.Categoria, 
                Duracion: _this.pelicula.Duracion, Pais: _this.pelicula.Pais},

            })
            .done(function(data) {

              alert( "Creado la pelicula -> " + "Id: " + data.Id + " Nombre: " + data.Nombre + " Director: " + data.Director 
                + " Categoria: " + data.Categoria + "Duracion" + data.Duracion + "Pais" + data.Pais);

              _this.refreshList();
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;
              

          })
          .fail(function(data) {
              alert( "ERROR, No se ha podido crear la Pelicula" );
          });

        },

        eliminarPelicula: function()
        {
             var _this = this;

            $.ajax({

              type: "DELETE",
              url: "http://localhost:51673/api/Peliculas/" + _this.pelicula.Id,
              data: { Id: _this.pelicula.Id }

            })
            .done(function(data) {

              alert( "Eliminado la pelicula -> " + "Id: " + data.Id + " Nombre: " + data.Nombre + " Director: " + data.Director 
                + " Categoria: " + data.Categoria + "Duracion" + data.Duracion + "Pais" + data.Pais);

              _this.refreshList();
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;

            })
            .fail(function(data) {
              alert( "ERROR, al eliminar la Pelicula" );
          });

        },

        actualizar: function()
        {
            var _this = this;

            $.ajax({

                type: "PUT",
                url: "http://localhost:51673/api/Peliculas/" + _this.pelicula.Id,
                data: _this.pelicula,

            })
            .done(function(data) {

                alert( "Se ha actualizado la Pelicula");
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
                alert( "ERROR, al actualizar la Pelicula" );
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
                  url : "http://localhost:51673/api/Peliculas/" + id,
                  type: "GET",
                })
                .done(function(data) {

                  _this.pelicula.Id = data.Id;
                  _this.pelicula.Nombre = data.Nombre;
                  _this.pelicula.Director = data.Director;
                  _this.pelicula.Categoria = data.Categoria;
                  _this.pelicula.Duracion = data.Duracion;
                  _this.pelicula.Pais = data.Pais;

                })
                .fail(function(data) {
                        alert( "ERROR, no se han podido recuperar los registros" );
                      });
          },

          refreshList: function()
          {

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
                alert( "ERROR, al refrescar" );
            });

          },

        limpiarCampos: function()
        {
            this.pelicula = {Id: "", Nombre: "", Director: "", Categoria: "", Duracion: "", Pais: "" };
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
                url : "http://localhost:51673/api/Peliculas",
                type: "GET",
            })
            .done(function(data) {
                _this.peliculas = data;
            })
            .fail(function(data) {
                alert( "ERROR, al iniciar los datos" );
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