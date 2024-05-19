<template>
    <h4>YOUR BALANCE</h4>
    <h1 id="balance">${{total}}</h1>
</template>

<script setup>
import {defineProps} from 'vue';
defineProps({
    total:{
        type: Number,
        require: true
    }
})
</script>