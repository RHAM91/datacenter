<template>
    <div class="contenedor_orden_compra">
        <div class="cuadro_orden_compra">

        </div>
    </div>
</template>

<script>
export default {
    name: 'OrdenCompra',
    data() {
        return {
            
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
            this.$emit('ocmodal', false)
        },
        doCommand(e){
            if (e.keyCode == 27) {
                this.salir()
            }
        },
    },
}
</script>

<style>
    .contenedor_orden_compra{
        width: 100%;
        height: 100vh;
        background-color: rgba(0, 0, 0, .3);
        position: fixed;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 999;
    }
        .cuadro_orden_compra{
            width: 700px;
            height: 600px;
            background-color: white;
            border-radius: 4px;
        }
</style>