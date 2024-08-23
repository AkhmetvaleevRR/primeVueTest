<template>
  <h1>Многострочный текст в ячейке</h1>
  <DataTable :value="logData" tableStyle="max-width: max-content">
      <Column header="Данные массива" >
        <template #body="slotProps">
          <p class="log-text">
            {{ slotProps.data.str }}
          </p>
      </template>
      </Column>
  </DataTable>
</template>

<script setup lang="ts">
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import { ref } from 'vue';

type LogEntry = {
  time: string;
  value: string;
};

const logStr = '[13:36:53] Расчетное время: 9 мин[13:36:58] Открыть клапан откачки К1[13:36:58] Включить вакуумный насос[13:36:58] Закрыть клапан К5[13:36:58] Закрыть клапан дистилляции К2[13:36:58] Ожидание: 8 с[13:37:06] Заливка 2.2мл. в испаритель[13:37:06] Заданно 26.50602409638554 шагов[13:37:09] Заливка перекиси завершена[13:37:09] Открыть клапан дистилляции К2[13:42:09] Включить нагрев испарителя[13:42:09] Закрыть клапан дистилляции К2[13:42:09] конечное давление1.0960040758227925 торр[13:42:09] Выпаривание через К2[13:42:09] Выпаривание длилось5 мин[13:42:09] Откачка до 1 торр[13:42:15] Закрыть клапан откачки К1[13:43:09] Открыть клапан откачки К1[13:43:14] Аппаратное смещение 0 денсит. = -0.313683180809021[13:43:14] Закрыть клапан дистилляции К2'
const parseLog = (log: string): string => {
  const logEntries: LogEntry[] = [];
  
  // Регулярное выражение для поиска времени и значения
  const logPattern = /(\[.{8}]) (.*?)(?=\[.{8}]|\s*$)/g;
  
  let match;
  while ((match = logPattern.exec(log)) !== null) {
    const [, time, value] = match;
    logEntries.push({ time: time.slice(1, -1), value });
  }
  
  return logEntries.map(entry => `[${entry.time}] ${entry.value}`).join('\n');
}

const logData = [{
  str: ref(parseLog(logStr))
}]
</script>

<style scoped lang='scss'>
.log-text {
  white-space: break-spaces;
}
</style>