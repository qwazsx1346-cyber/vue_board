<script setup>
import { reactive, onMounted } from 'vue';
import { useRoute } from 'vue-router'; //path variable데이터를 받을 때 필요객체

const route = useRoute();

onMounted(() => {
    console.log('route.params.id: ', route.params.id);
})
</script>

<template>
<h3>Detail</h3>
</template>

<style scoped>

</style>