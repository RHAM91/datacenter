<template>
    <div class="info">
        <pre>
            {{carrito}}
        </pre>
    </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
    name: "Carrito",
    computed:{
        ...mapState(['carrito'])
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