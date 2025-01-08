<script setup lang="ts">
import { utils, writeFile } from "xlsx";

const { data } = defineProps<{
  data: any[];
}>();

function exportToExcel() {
  try {
    // Create a worksheet
    const worksheet = utils.json_to_sheet(data);

    // Set column widths
    worksheet["!cols"] = [
      { wch: 20 },
      { wch: 35 },
      { wch: 15 },
      { wch: 15 },
      { wch: 15 },
    ];

    // Bold the header row (first row)
    const headers = Object.keys(data[0] || {});
    headers.forEach((_, index) => {
      const cellAddress = utils.encode_cell({ r: 0, c: index }); // Row 0, each column
      if (worksheet[cellAddress]) {
        worksheet[cellAddress].s = {
          font: { bold: true }, // Apply bold style
        };
      }
    });

    // Create a workbook
    const workbook = utils.book_new();
    utils.book_append_sheet(workbook, worksheet, "Sheet1");

    // Generate Excel file and trigger download
    writeFile(workbook, "formatted_data.xlsx");

    alert("Excel file downloaded successfully!");
  } catch (error) {
    alert("An error occurred while exporting the Excel file.");
  }
}
</script>
<template>
  <button
    @click="exportToExcel"
    class="flex flex-col justify-center items-center w-full pt-10"
  >
    <img class="w-32 h-" src="/plane-icon.svg" alt="hello" />
    Export JSON to Excel
  </button>
</template>
