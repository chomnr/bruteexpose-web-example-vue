<template>
  <div>
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs';

import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BruteLoggerChart',
  components: { Bar },
  data() {
    return {
      chartData: {
        labels: ['UNDEFINED'],
        datasets: [
          {
            label: 'UNDEFINED',
            backgroundColor: '#661b1c',
            data: [0]
          }
        ]
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: true,
      }
    };
  },
  mounted() {
    this.loadCustom(["Loading..."], [0])
  },
  methods: {
    loadCustom(labels = [], data = [], ) {
      this.chartData = {
        labels: labels,
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#661b1c',
            data: data
          }
        ]
      };
    }
  }
};

</script>
