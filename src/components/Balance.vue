<template>
    <h4>Your Balance</h4>
    <h1 id="balance":class="total < 0 ? 'font-color-red' : 'font-color-green'" >${{ total }} </h1>
    <h4>Your Tax (30%)</h4>
    <h1 :class="'font-color-red'"> ${{ income*(30/100) }}</h1>
    
</template>
<script setup>
import { defineProps } from 'vue';
const props = defineProps({
    total:{
        type:Number,
        required:true,
    },
    income:{
type : Number,
required : true,
    }
});
</script>