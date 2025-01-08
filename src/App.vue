<script setup lang="ts">
import { onMounted, ref } from "vue";
import { read, utils, writeFile } from "xlsx";

import ExportToExcel from "./components/ExportToExcel.vue";

const data = ref([]);
const isLoading = ref(false);
const getData = async () => {
  isLoading.value = true;
  try {
    const response = await fetch("/data.json");
    // if (!response.ok) {
    //   throw new Error(`HTTP error! status: ${response.status}`);
    // }
    const jsonData = await response.json();
    console.log(jsonData.data);
    data.value = jsonData.data;
  } catch (error) {
    console.error("Error fetching the JSON data:", error.message);
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  getData();
});
</script>

<template>
  <main class="p-8 h-screen bg-gradient-to-br from-blue-500 via-indigo-500 to-purple-500 text-white">
    <div
      v-if="isLoading"
      class="flex items-center justify-center h-screen w-full text-lg font-semibold mt-4"
    >
      <span
        class="loader animate-spin border-4 border-t-transparent border-blue-500 rounded-full w-8 h-8 mr-2"
      ></span>
      Loading...
    </div>
    <div v-else>
      <h1 class="text-3xl font-bold">Click the button and get the Excel file</h1>
      <ExportToExcel :data="data" />
    </div>
  </main>
</template>

<style scoped></style>
