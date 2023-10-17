<template>
  <div>
    <button @click="add">Add</button>
    <table class="table" style="display: block; max-height: 200px; overflow: auto;" ref="tableRef">
      <thead>
        <tr>
          <th v-for="(column, index) in columns" :key="index">{{ column }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in rows" :key="rowIndex">
          <td v-for="(column, columnIndex) in columns" :key="columnIndex">{{ row[column as keyof TableRow] }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { ref, onMounted, Ref } from 'vue';
import { TableRow } from './types';

export default {
  name: 'DataTable',
  setup() {
    const index = ref<number>(3);
    const columns: string[] = ['name', 'type', 'description'];

    const rows: Ref<TableRow[]> = ref([
      {
        name: 'Name1',
        type: 'Type1',
        description: 'Description1',
      },
      {
        name: 'Name2',
        type: 'Type2',
        description: 'Description2',
      },
    ]);

    const tableRef: Ref<HTMLTableElement | null> = ref(null);

    const add = () => {
      rows.value.push({
        name: 'Name' + index.value,
        type: 'Type' + index.value,
        description: 'Description' + index.value,
      });
      scrollToBottom();
      index.value++;
    };

    const scrollToBottom = () => {
      if (tableRef.value) {
        tableRef.value.scrollTop = tableRef.value.scrollHeight;
      }
    };

    onMounted(() => {
      scrollToBottom();
    });

    return {
      columns,
      rows,
      add,
      tableRef,
    };
  },
};
</script>
