<script setup lang="ts">
import dateTimePicker from '@/components/dateTimePicker.vue';
import linkInput from '@/components/linkInput.vue';
import multiSelect from '@/components/multiSelect.vue';
import tableData from '@/components/tableData.vue';
import filterTable from '@/components/filterTable.vue';
</script>

<template>
  <main>
    <linkInput />
    <dateTimePicker />
    <multiSelect />
    <tableData />
    <filterTable />
  </main>
</template>
