<template>
    <div class="contenedor_less">
        <div class="cuadro_less">
            <b-container fluid>
                <b-row>
                    <b-col sm="12" class="mt-3">
                        <h2>
                            Consulta de productos con poco stock
                        </h2>
                    </b-col>
                    <b-col sm="12" md="5" class="mt-3">
                        <label>Menor a</label>
                        <b-form-input type="number" v-model="cantidad" size="sm" @keydown.enter="getDatos"></b-form-input>
                    </b-col>
                    <b-col sm="12" md="7" class="mt-3">
                        <label>Bodega</label>
                        <select class="form-control form-control-sm" @change="getDatos" v-model="bodega">
                            <option value="">Selecciona</option>
                            <option value="iglesia">Iglesia</option>
                            <option value="oficina">Oficina</option>
                        </select>
                    </b-col>
                    <!-- <b-col sm="12" md="2" class="mt-5">
                        <b-button type="button" size="sm" block variant="success" @click="getDatos">Consultar</b-button>
                    </b-col> -->
                    <b-col v-if="bodega == 'iglesia'" sm="12" class="mt-3">
                        <div class="datos_resultado">
                            <table class="table table-bordered table-hover table-sm" style="font-size: 12px;">
                                <thead>
                                    <tr>
                                        <th style="width: 25%;">
                                            Código
                                        </th>
                                        <th style="width: 65%;">
                                            Nombre
                                        </th>
                                        <th style="width: 10%;text-align: center;">
                                            Iglesia
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in dataIglesia" :key="index">
                                        <td>
                                            {{item.codigo}}
                                        </td>
                                        <td>
                                            {{item.nombre}}
                                        </td>
                                        <td :class="[item.existencia_iglesia == 0 ? '_rojo': item.existencia_iglesia == 1 ? '_anaranjado': item.existencia_iglesia == 2 ? '_amarillo': '']" style="text-align: center;">
                                            {{item.existencia_iglesia}}
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </b-col>
                    <b-col v-if="bodega =='oficina'" sm="12" class="mt-3">
                        <div class="datos_resultado">
                            <table class="table table-bordered table-hover table-sm" style="font-size: 12px;">
                                <thead>
                                    <tr>
                                        <th style="width: 25%;">
                                            Código
                                        </th>
                                        <th style="width: 65%;">
                                            Nombre
                                        </th>
                                        <th style="width: 10%;text-align: center;">
                                            Oficina
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in dataOficina" :key="index">
                                        <td>
                                            {{item.codigo}}
                                        </td>
                                        <td>
                                            {{item.nombre}}
                                        </td>
                                        <td :class="[item.existencia_oficina == 0 ? '_rojo': item.existencia_oficina == 1 ? '_anaranjado': item.existencia_oficina == 2 ? '_amarillo': '']" style="text-align: center;">
                                            {{item.existencia_oficina}}
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </b-col>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>

import axios from 'axios'
import {IP, PUERTO} from '@/config/parametros'
import { minix } from '@/components/functions/alertas'

export default {
    name: 'Less',
    data() {
        return {
            cantidad: 3,
            bodega: '',
            dataOficina: [],
            dataIglesia: []
        }
    },
    created() {
        window.addEventListener('keydown', this.doCommand)
    },
    destroyed() {
        window.removeEventListener('keydown', this.doCommand)
    },
    methods: {
        salir(){
            this.$emit('clsModal', false)
        },
        doCommand(e){
            if (e.keyCode == 27) {
                this.salir()
            }
        },
        async getDatos(){

            if (this.cantidad == '' || this.cantidad <= 0 || this.cantidad == '0') {
                minix({icon: 'error', mensaje: 'La cantidad a consultar debe ser mayor que cero', tiempo: 3000})
            }else{

                if (this.bodega == 'iglesia') {
                    
                    let data = {
                        bodega: this.bodega,
                        cantidad: this.cantidad
                    }
        
                    let datosIglesia = await axios.post(`http://${IP}:${PUERTO}/api/productos/less`, data, this.$store.state.token)
                    this.dataIglesia = datosIglesia.data
                }else{
    
                    let data = {
                        bodega: this.bodega,
                        cantidad: this.cantidad
                    }
        
                    let datosOficina = await axios.post(`http://${IP}:${PUERTO}/api/productos/less`, data, this.$store.state.token)
                    this.dataOficina = datosOficina.data
                    
                }
            }
            

        }
    },
    mounted() {
    },
}
</script>

<style>
    .contenedor_less{
        width: 100%;
        height: 100vh;
        background-color: rgba(0, 0, 0, 0.3);
        position: fixed;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 999;
    }
        .cuadro_less{
            width: 800px;
            height: 600px;
            background-color: white;
            border-radius: 4px;
        }

        .datos_resultado{
            width: 100%;
            height: 425px;
            overflow: auto;
        }

        ._rojo{
            background-color: #ef3054;
            color: white;
        }
        
        ._amarillo{
            background-color: #ffe347;
            color: black;
        }
        ._anaranjado{
            background-color: #ffb400;
            color: white;
        }
</style>