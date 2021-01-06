<template>
    <b-container fluid>
        <b-row>
            <b-col sm="12" class="mt-3">
                <table class="table table-sm table-bordered table-hover" style="font-size: 12px;">
                    <thead>
                        <tr>
                            <td>
                                No. Orden
                            </td>
                            <td>
                                Estado
                            </td>
                            <td>
                                Fecha
                            </td>
                            <td>
                                ...
                            </td>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in inventario_ordenes" :key="index">
                            <td>
                                {{item._id}}
                            </td>
                            <td>
                                {{item.aprobada}}
                            </td>
                            <td>
                                {{item.fecha}}
                            </td>
                            <td>

                            </td>
                        </tr>
                    </tbody>
                </table>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
import { mapState } from 'vuex'
export default {
    name: 'Ordenes',
    filters:{
        aprobado: function(val){
            if(val == ''){
                return 'Aprobado'
            }else{
                return 'Pendiente'
            }
        }
    },
    computed:{
        ...mapState(['inventario_ordenes'])
    },
    data() {
        return {
            
        }
    },
    methods: {
        
    },
}
</script>

<style>

</style>