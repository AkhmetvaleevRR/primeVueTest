<template>
  <h1>DateTime picker</h1>
  <Calendar v-model="time" showIcon timeOnly showButtonBar />
</template>

<script setup lang="ts">
import Calendar from 'primevue/calendar';
import { ref } from 'vue';
const time= ref('')
</script>

<style scoped lang='scss'>
</style>