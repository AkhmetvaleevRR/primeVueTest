<template>
  <div>
    <div class="card">
      <DataTable
        v-model:value="filteredData"
        dataKey="id"
        :paginator="true"
        :rows="10"
        paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"
        :rowsPerPageOptions="[5, 10, 25]"
        currentPageReportTemplate="Showing {first} to {last} of {totalRecords} products"
      >
        <template #header>
          <div>
            <h1 class="m-0">Таблица с поиском (расширенное задание)</h1>
            <div class="table-header">
              <InputGroup class="table-header__search">
                <Button icon="pi pi-sliders-v" severity="success" @click="toggleDropdown"/>
                <Listbox 
                  v-model="selectedFilter"
                  :options="columnValue" multiple optionLabel="label" 
                  class="dropdown-listbox"
                  :style="{ display: showList ? 'block' : 'none' }" 
                />
                <InputText 
                v-model="searchQuery" 
                placeholder="Search ..."  
                @keypress.enter="searchData"
                />
                <Button v-if='searchQuery' icon="pi pi-times" severity="danger" class="" @click="searchQuery=''"/>
                <Button severity="info" @click="searchData">Search</Button>
              </InputGroup>
              <MultiSelect 
                v-model="columnValue" 
                class="table-header__select"
                :options="columns" 
                optionLabel="label" filter 
                placeholder="Поля таблицы"
                :maxSelectedLabels="2"
              >
                <template #value>
                  
                </template>
              </MultiSelect>
            </div>
          </div>      
        </template>
        <Column 
          v-for="col of columnValue" 
          :key="col.field" 
          :field="col.field" 
          :header="col.label" 
          :sortable="col.sortable"
        >
          <template v-if="col.type === 'review'" #body="slotProps">
            <Rating :modelValue="slotProps.data.rating" :readonly="true" :cancel="false" />
          </template>
          <template v-if="col.type === 'status'" #body="slotProps">
            <Tag :value="slotProps.data.inventoryStatus" :severity="getStatusLabel(slotProps.data.inventoryStatus)" />
          </template>
        </Column>
      </DataTable>
    </div>    
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import MultiSelect from 'primevue/multiselect';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import InputText from 'primevue/inputtext';
import InputGroup from 'primevue/inputgroup';
import Listbox from 'primevue/listbox';
import Button from 'primevue/button';
import Rating from 'primevue/rating';
import Tag from 'primevue/tag';

interface iColumn {
  field: string
  label: string
  sortable: boolean
  type: string
}

const columnValue = ref<iColumn[]>([
  { field: 'code', label: 'Code', type:'text', sortable: true },
  { field: 'name', label: 'Name', type:'text', sortable: true },
  { field: 'image', label: 'Image', type:'text', sortable: false },
  { field: 'price', label: 'Price', type:'text', sortable: true },
  { field: 'category', label: 'Category', type:'text', sortable: true },
  { field: 'reviews', label: 'Reviews', type:'review', sortable: true },
  { field: 'status', label: 'Status', type:'status', sortable: true }
])

const columns = [
    { field: 'code', label: 'Code' },
    { field: 'name', label: 'Name' },
    { field: 'image', label: 'Image' },
    { field: 'price', label: 'Price' },
    { field: 'category', label: 'Category' },
    { field: 'reviews', label: 'Reviews' },
    { field: 'status', label: 'Status' }
];

const products = ref([{
    id: '1000',
    code: 'f230fh0g3',
    name: 'Bamboo Watch',
    description: 'Product Description',
    image: 'bamboo-watch.jpg',
    price: 65,
    category: 'Accessories',
    quantity: 24,
    inventoryStatus: 'INSTOCK',
    rating: 5
    },
    {
    id: '1000',
    code: 'f230fh0g4',
    name: 'Wooden Watch',
    description: 'Product Description',
    image: 'Wooden-watch.jpg',
    price: 75,
    category: 'Accessories',
    quantity: 24,
    inventoryStatus: 'LOWSTOCK',
    rating: 4
    }
  ] 
)

const showList = ref(false);

const toggleDropdown = () => {
  showList.value = !showList.value;
};
const filteredData = ref(products.value)

const searchData = () => {
  if (!searchQuery.value) {
    return filteredData.value = products.value;
  }
  
  return filteredData.value = products.value.filter(item => {
    return selectedFilter.value.some(filter => {
      const fieldValue = item[filter.field];
      return fieldValue !== undefined &&
             fieldValue.toString().toLowerCase().includes(searchQuery.value.toString().toLowerCase());
    });
  });
};


const searchQuery = ref('')

const selectedFilter = ref<iColumn[]>([])

const getStatusLabel = (status: any) => {
  switch (status) {
    case 'INSTOCK':
      return 'success'

    case 'LOWSTOCK':
      return 'warn'

    case 'OUTOFSTOCK':
      return 'danger'

    default:
      return null
  }
}

</script>

<style scoped lang="scss">
.table {
  &-header {
      position: relative;
      display: flex;
      width: 100%;
      gap: 10px;
      &__search {
        width: 80%;
      }
      &__select {
        width: 20%;
        height: 42px;
      }
  }
}

.dropdown-listbox {
  position: absolute;
  bottom: 42px;
  left: 0;
  width: 200px; 
  z-index: 1000;
}
</style>