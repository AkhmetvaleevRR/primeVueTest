<template>
  <div>
    <h1>Поле Link</h1>    
    <div class="fake-input" @click="title = ''">
      <a v-if="title" :href="inputValue">{{ title }}</a>
      <InputText
        v-else
        class="fake-input__input"
        v-model="inputValue"
        @focus="handleFocus"
        @blur="handleBlur"
        @keypress.enter="handleEnter"
        placeholder="https://"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import InputText from 'primevue/inputtext';
import { ref } from 'vue';
const inputValue = ref('https://')
const title = ref('')

import axios from 'axios';
import * as cheerio from 'cheerio';

const handleFocus = () => {
  if (!inputValue.value) {
    inputValue.value = 'https://'
  }
};

function isValidUrl(url: string): boolean {
  try {
    new URL(url);
    return true;
  } catch (_) {
    return false;
  }
}

const fetchTitle = async (url: string) => {
  if (isValidUrl(url)) {
    try {
      const response = await axios.get(url);
      const $ = cheerio.load(response.data);
      const pageTitle = $('title').text().trim();
      title.value = pageTitle;
    } catch (error) {
      console.error(`Ошибка при получении заголовка:`, error);
    }
  }
  else {
    return console.error(`Введенная строка не является url`);
  }
};


const handleEnter = () => {
  if (inputValue.value) {
    fetchTitle(inputValue.value);
  }
};

const handleBlur = () => {
  if (inputValue.value) {
    fetchTitle(inputValue.value);
  }
};

</script>

<style scoped lang='scss'>
.fake-input {
  width: 200px;
  font-family: inherit;
  font-feature-settings: inherit;
  font-size: 1rem;
  color: var(--p-inputtext-color);
  background: var(--p-inputtext-background);
  padding: var(--p-inputtext-padding-y) var(--p-inputtext-padding-x);
  border: 1px solid var(--p-inputtext-border-color);
  transition: background var(--p-inputtext-transition-duration), color var(--p-inputtext-transition-duration), border-color var(--p-inputtext-transition-duration), outline-color var(--p-inputtext-transition-duration), box-shadow var(--p-inputtext-transition-duration);
  appearance: none;
  border-radius: var(--p-inputtext-border-radius);
  outline-color: transparent;
  box-shadow: var(--p-inputtext-shadow);
  &__input {
    border: 0;
    background-color: transparent;
    padding: 0;
  }
}
</style>