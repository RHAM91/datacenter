<template>
    <div class="contenedor-principal">

            <aside class="aside" id="aside">
                <!-- Estoy ejecutando directamente la funcion ToggleMenu para cerrar el menu -->
                <Menu 
                    v-on:establecermodulo="setmodulo"
                />
            </aside>


        <div class="lienzo" id="lienzo">
            <div class="cuerpo" id="cuerpo">
                <!-- AQUI VAN LOS COMPONENTES -->
               

                <div v-if="modulo == 'inventario-sistemas'">
                    <InventarioTabs v-on:OpenMenu="ToggleMenu" />
                </div>

                <div v-if="modulo == 'reporte-ingresos'">
                    <ReporteTabsKardex v-on:OpenMenu="ToggleMenu"/>
                </div>

                <div v-if="modulo == 'pedidos'">
                    <PedidosTabs v-on:OpenMenu="ToggleMenu"/>
                </div>

                <div v-if="modulo == 'ordendecompra'">
                    <OrdenCompraTabs v-on:OpenMenu="ToggleMenu"/>
                </div>

                <div v-if="modulo == 'configuracion'">
                    <ConfiguracionTabs v-on:OpenMenu="ToggleMenu"/>
                </div>

            </div> 

        </div>

        <Loading />

    </div>
</template>

<script>

// COMPONENTES


// MODULOS
import InventarioTabs from '@/components/Inventarios/Tabs.vue'
import ReporteTabsKardex from '@/components/Reportes/Kardex/Tabs.vue'
import PedidosTabs from '@/components/Pedidos/Tabs.vue'
import OrdenCompraTabs from '@/components/OrdenesDeCompra/Tabs.vue'
import ConfiguracionTabs from '@/components/Configuracion/Tabs.vue'


//--> GIF ANIMADO PARA LAS DESCARGAS

import Loading from '@/components/varios/Loading.vue'

// MODULO PRINCIPAL

import Menu from '@/components/menu/Menu.vista.vue'

import axios from 'axios'
import { IP, PUERTO } from '../config/parametros'
import { mapActions } from 'vuex'

export default {
    name: 'Main',
    components:{
        Menu,
        Loading,
        InventarioTabs,
        ReporteTabsKardex,
        PedidosTabs,
        ConfiguracionTabs,
        OrdenCompraTabs
    },
    data(){
        return{
            conexion: null,
            mostrarMenu: true,
            modulo: ''
        }
    },
    methods: {
        consulta: async function(){

            let info = await axios.get(`http://${IP}:${PUERTO}/api/auth/login`, this.$store.state.token)
            this.conexion = info.data.alive

            if (this.conexion == false) {
                this.$cron.stop('consulta')
                localStorage.removeItem('kat')
                this.$router.replace('login')
            }

        },
        setmodulo(modulo){

            this.modulo = modulo
        },
        ToggleMenu(valor){ // el parametro "valor" no tiene ninguna funcion de momento
            
            this.mostrarMenu = !this.mostrarMenu

             if (this.mostrarMenu) {
                document.getElementById('aside').classList.remove('newSize')
                document.getElementById('lienzo').classList.remove('lienzo-100')
            }else{
                document.getElementById('aside').classList.add('newSize')
                document.getElementById('lienzo').classList.add('lienzo-100')
            }
            
        },
        menu_fijo: function(){

            document.getElementById('cuerpo').onscroll = function (){

                var scroll = document.getElementById('cuerpo').scrollTop || document.getElementById('cuerpo').scrollTop;
                
                if(scroll >= 15){                    
                    document.getElementById('navb').classList.add('fijar')
                }else{
                    document.getElementById('navb').classList.remove('fijar')
                }
            }
        },
        ...mapActions(['ws'])
         
    },
    mounted() {
        // CONEXION CON EL SERVIDOR PUSH PARA ESCUCHAR ACTUALIZACIONES

        const { socket } = require('../config/sockets_config')
        this.ws(socket)

        // TERMINA FUNCION -->


        this.menu_fijo()
        
        this.$store.dispatch('getPermisos', {
            token: this.$store.state.token,
            rutas:[
                'pacientes',
                'instituciones',
                'medicos',
                'departamentos',
                'subir_archivos',
                'odt',
                'operarios',
                'aparatos',
                'reportes',
                'abonos',
                'aparatos',
                'formulas',
                'formulaElementos'
            ]
        })
        
    },
    cron:{
        time: 120000,
        method: 'consulta'
    }
}

</script>

<style>


    .contenedor-principal{
        width: 100%;
        height: 100vh;
        display: flex;
        
    }

        .aside{
            width: 250px;
            height: 100%;
            background-color: rgb(6, 30, 51);
            transition: .4s;
            
        }

        .newSize{
            transform: translateX(-250px);
            width: 0px;
            transition: .5s ;
            
        }

        .lienzo{
            width: calc(100% - 250px);
            height: 100%;
            position: relative;
            transition: .4s ;
        }

        .lienzo-100{
            width: 100%;
            transition: .4s ;
        }


/* ESTILOS PARA MODIFICACION */

.fijar{
    z-index: 999;
    box-shadow: 0px 1px 2px 0px rgba(60,64,67,.3), 0px 1px 3px 1px rgba(60,64,67,.15);
}


    /* ESTILOS DEL COMPONENTE DE TEMPLATE.VISTA.VUE */

            

            .cuerpo{
                width: 100%;
                height: 100%;
                overflow: auto;
                position: relative;
            }

            

/* ESTILOS PARA LA BARRA DE SCROLL */

::-webkit-scrollbar{
    width: 8px;
    background-color: #F5F5F5;
}
::-webkit-scrollbar-track{
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    background-color: #F5F5F5;
}
::-webkit-scrollbar-thumb{
    /* background-color: #F90; // naranja  */ 
    background-color: #29B6F6;
    background-image: -webkit-linear-gradient(90deg,rgba(255, 255, 255, .2) 25%,transparent 25%,transparent 50%,rgba(255, 255, 255, .2) 50%,rgba(255, 255, 255, .2) 75%,transparent 75%,transparent)
}

    
</style>