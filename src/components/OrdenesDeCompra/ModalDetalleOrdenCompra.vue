<template>
    <div class="contenedor_etrada_producto">
        <div class="formulario_entrada_producto2">
            <div class="cabecera_formulario_entrada_producto">

            </div>
            <b-container fluid>
                <b-row>
                    <b-col sm="12" class="mt-3">
                        <h2>
                            Detalle orden # {{idproducto}}
                        </h2>
                    </b-col>
                    <b-col sm="12" class="mt-1">
                        <p>
                            {{justificacion}}
                        </p>
                    </b-col>
                    <b-col sm="12" class="mt-3">
                        <table class="table table-sm table-bordered table-striped" style="font-size: 12px;">
                            <thead>
                                <tr>
                                    <th>
                                        Código
                                    </th>
                                    <th>
                                        Nombre
                                    </th>
                                    <th style="text-align: center;">
                                        Proveedor
                                    </th>
                                    <th style="text-align: center;">
                                        Para
                                    </th>
                                    <th style="text-align: center;">
                                        Cantidad
                                    </th>
                                    <th style="text-align: center;">
                                        Precio
                                    </th>
                                    <th style="text-align: center;">
                                        Subtotal
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in detalleOrden" :key="index">
                                    <td>
                                        {{item.codigo}}
                                    </td>
                                    <td>
                                        {{item.nombre}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.proveedor}}
                                    </td>
                                    <td style="text-align: center;">
                                        {{item.bodega}}
                                    </td>
                                    <td style="text-align: center;">    
                                        {{item.cantx}}
                                    </td>
                                     <td style="text-align: center;">    
                                        Q{{item.precio}}
                                    </td>
                                     <td style="text-align: center;">    
                                        Q{{item.subtotal}}
                                    </td>
                                </tr>
                                <tr>
                                    <td></td>
                                    <td></td>
                                    <td></td>
                                    <td></td>
                                    <td></td>
                                    <td style="text-align: center;background-color: black;color: white;">Total</td>
                                    <td style="text-align: center;">
                                        Q{{total}}
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </b-col>
                </b-row>
            </b-container>
            <div class="botonera">
                <b-container fluid>
                    <form @submit.prevent="enviarDocumentacion">
                        <b-row>
                            <b-col sm="10" class="mt-3">
                                <label>Comprobantes</label>
                                <b-form-file
                                    v-model="foto"
                                    placeholder="Elige un archivo o déjalo caer aquí..."
                                    drop-placeholder="Suelta el archivo aquí..."
                                    size="sm"
                                    required
                                    
                                ></b-form-file>
                            </b-col>
                            <b-col sm="2" class="mt-5">
                                <b-button type="submit" size="sm" block variant="success"><i class="fas fa-arrow-circle-up" style="margin-right: 10px;"></i>Subir</b-button>
                            </b-col>
                        </b-row>
                    </form>
                </b-container>
            </div>
        </div>
    </div>
</template>

<script>

import {IP, PUERTO} from '@/config/parametros'
import { minix } from '../functions/alertas'
import Pacman from '@/components/varios/_Loading.vue'
import axios from 'axios'
import { mapMutations } from 'vuex'

export default {
    name: 'DetalleOrden',
    props:['idproducto'],
    components:{
        Pacman
    },
    data() {
        return {
            foto: null,
            detalleOrden: [],
            justificacion: '',
            total: 0
        }
    },
    created() {
        window.addEventListener('keydown', this.doCommand)
    },
    destroyed() {
        window.removeEventListener('keydown', this.doCommand)
    },
    methods: {
        limpiar(){
            this.foto = null
        },
        cerrar(){
            this.$emit('cerrarModalDet', false)
        },
        doCommand(e){
            if (e.keyCode == 27) {
                this.cerrar()
            }
        },
        async getdetalle(){
            let datos = await axios.get(`http://${IP}:${PUERTO}/api/ordenes/${this.idproducto}`, this.$store.state.token)
            this.detalleOrden = JSON.parse(datos.data.detalle)
            this.justificacion = datos.data.comentarios

            let contador = 0

            this.detalleOrden.forEach(e => {
                contador += e.subtotal
            });

            this.total = contador
        },
        async enviarDocumentacion(){

            let formData = new FormData()

            formData.append('image', this.foto)
            formData.append('id', this.idproducto)

            this.set_loading_(true)

            let upl = await axios.post(`http://${IP}:${PUERTO}/api/ordenes/docs`, formData, this.$store.state.token)

            this.set_loading_(false)
            
            if (upl.data == 'e001') {
                minix({icon: 'error', mensaje: 'Solo archivos .pdf son permitidos', tiempo: 4000})
                this.set_loading_(false)
            }else if (upl.data.message == 'OK') {
                minix({icon: 'success', mensaje: 'Artículo creado correctamente', tiempo: 3000})
                this.set_loading_(false)
                this.limpiar()
            }else if(upl.data.message == 'e002'){
                minix({icon: 'error', mensaje: 'El archivo no puede ser mayor a 5Mb de tamaño', tiempo: 4000})
                this.set_loading_(false)
            }else{
                minix({icon: 'info', mensaje: upl.data.message, tiempo: 3000})
                this.set_loading_(false)
            } 

        },
        ...mapMutations(['set_loading_'])
    },
    mounted() {
        this.getdetalle()
    },
}
</script>

<style>
    .formulario_entrada_producto2{
        width: 750px;
        height: 600px;
        background-color: white;
        position: relative;
    }
        .botonera{
            width: 100%;
            height: 110px;
            position: absolute;
            left: 0;
            bottom: 0;
        }
</style>