<template>
    <b-container fluid>
        <b-row>
            <b-col sm="12" class="mt-3">
                <b-button type="button" size="sm" variant="success" @click="abriModal_less">Less</b-button>
            </b-col>
            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-bordered table-hover" style="font-size: 12px;">
                    <thead>
                        <tr>
                            <th style="width: 25%;">
                                Código
                            </th>
                            <th style="width: 55%;">
                                Nombre
                            </th>
                            <th style="width: 10%;text-align: center;">
                                Bodega
                            </th>
                            <th style="width: 10%;text-align: center;">
                                Cantidad
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in carrito" :key="index">
                            <td>
                                {{item.codigo}}
                            </td>
                            <td>
                                {{item.nombre}}
                            </td>
                            <td style="text-align:center;">
                                {{item.bodega}}
                            </td>
                            <td>
                                
                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>

        <Less v-if="modal_less" v-on:clsModal="cerrarModal_less" />
    </b-container>
</template>

<script>
import { mapState } from 'vuex'
import Less from './Less.vue'

export default {
    name: "Carrito",
    components:{
        Less
    },
    computed:{
        ...mapState(['carrito'])
    },
    data() {
        return {
            modal_less: false
        }
    },
    methods: {
        abriModal_less(){
            this.modal_less = true
        },
        cerrarModal_less(){
            this.modal_less = false
        }
    },
}
</script>

<style>

</style>