<template>
    <div class="absolute group transition-transform duration-300 hover:scale-105" :style="style">
        <img :src="ori" class="absolute inset-0 transition-opacity duration-300 group-hover:opacity-0" />
        <img :src="hover" class="absolute inset-0 opacity-0 transition-opacity duration-300 group-hover:opacity-100" />
    </div>
</template>

<script setup>
defineProps({
    ori: String,
    hover: String,
    style: Object
})
</script>
