<template>
   
    <div class='cart-section d-flex align-items-center'>
        
        <i class="fas fa-shopping-cart fa-2x"></i>
        
        <h5 class='px-2'>
            ({{itemsQuantity}}) 
        </h5>

        <h5 class='px-2'>
            {{itemsTotalCost}}$
        </h5>

    </div>

</template>

<script setup>

    import { defineProps } from 'vue'

    defineProps({
       'itemsQuantity': Number,
       'itemsTotalCost': Number
    })

</script>

<style scoped>

    .cart-section{
        cursor: pointer;
    }

</style>
