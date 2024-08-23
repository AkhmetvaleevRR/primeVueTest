<template>
  <h1>Сброс значений в Select </h1>
  <MultiSelect 
    v-model="selectedCities" 
    :options="cities" 
    optionLabel="name" 
    class="select-class"
    placeholder="Select Cities"
    :maxSelectedLabels="3"
  > <template #value>
      {{nameList}}
      <button v-if="selectedCities.length>0" class='clear-button'  @click="selectedCities = []">
        <i class="pi pi-times"></i>
      </button> 
    </template>
  </MultiSelect>
</template>

<script setup lang="ts">
import MultiSelect from 'primevue/multiselect';
import { ref, computed } from 'vue';
const selectedCities = ref([])

const nameList = computed(() => {
  return selectedCities.value.map((obj: { name: string; }) => obj.name).join(',')
})

const cities = [
  {
    name: 'london'
  },
  {
    name: 'paris'
  },
  {
    name: 'moscow'
  },

]
</script>

<style scoped lang='scss'>
.clear-button {
  background-color: transparent;
  border: 0;
  margin-left: auto;
  margin-right: -20px;
}
.select-class {
  width: 250px;
  height: 32px;
}
</style>