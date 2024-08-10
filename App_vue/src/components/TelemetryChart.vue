<!-- Copyright (C) Tommy Minter 2024-->
<template class="chart-page">
  <!-- Container for charts -->
  <div class="chart-container">
      <div class="chart-row">
          <div class="mini-chart chart-border" ref="chart1"></div>
          <div class="mini-chart chart-border" ref="chart2"></div>
      </div>
      <div class="chart-row">
          <div class="mini-chart chart-border" ref="chart3"></div>
          <div class="mini-chart chart-border" ref="chart4"></div>
      </div>
  </div>

  <!-- Button to reload charts still dosn't do anything yet-->
  <button class="reload-button" @click="reloadCharts">Reload Charts</button>
</template>

<script setup>
import { onMounted, ref } from 'vue';

// References for each chart element
const chart1 = ref(null);
const chart2 = ref(null);
const chart3 = ref(null);
const chart4 = ref(null);

// Function to fetch chart data from the JSON file
async function fetchChartData() {
  try {
      const response = await fetch('/chart-data.json');
      if (!response.ok) {
          throw new Error('Failed to load chart data');
      }
      const data = await response.json();
      updateCharts(data);
  } catch (error) {
      console.error('Error loading chart data:', error);
  }
}

// Function to update charts with the fetched data
function updateCharts(data) {
  drawSpecificChart(chart1.value, data.chart1Data, 'BarChart');
  drawSpecificChart(chart2.value, data.chart2Data, 'PieChart');
  drawSpecificChart(chart3.value, data.chart3Data, 'ColumnChart');
  drawSpecificChart(chart4.value, data.chart4Data, 'LineChart');
}

// Function to draw a specific chart given a container, data options, and chart type
function drawSpecificChart(container, dataOptions, chartType) {
  const data = google.visualization.arrayToDataTable(dataOptions.data);
  let chart;

  if (chartType === 'BarChart') {
      chart = new google.visualization.BarChart(container);
  } else if (chartType === 'PieChart') {
      chart = new google.visualization.PieChart(container);
  } else if (chartType === 'ColumnChart') {
      chart = new google.visualization.ColumnChart(container);
  } else if (chartType === 'LineChart') {
      chart = new google.visualization.LineChart(container);
  }
  
  chart.draw(data, dataOptions.options);
}

// reload the charts still not sure how to hook this up properly
const reloadCharts = () => {
  fetchChartData(); // Re-fetch chart data and update charts
};

// Load Google Charts and fetch chart data when the component is mounted
onMounted(() => {
  google.charts.load('current', { 'packages': ['corechart'] });
  google.charts.setOnLoadCallback(fetchChartData);
});
</script>

<style scoped>
.chart-container {
  display: flex;
  flex-direction: column;
  gap: 1rem; 
}

.chart-row {
  display: flex;
  justify-content: space-between; 
  gap: 1rem; 
}

.mini-chart {
  width: 20rem;
  height: 20rem;
}

.chart-border {
  border: 2px solid white;
}

/* Style for the reload button */
.reload-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: rgb(0, 97, 65); /* trying to match the defualt Vue Green */
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

/* Hover effect for the reload button */
.reload-button:hover {
  background-color: #2e6631;
}
</style>