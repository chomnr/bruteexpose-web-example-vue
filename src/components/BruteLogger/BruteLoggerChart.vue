<template>
  <div>
    <Bar :data="chartData" :options="chartOptions" ref="bar" />
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
            label: 'Most Attacks By Country',
            backgroundColor: '#163531',
            data: [0]
          }
        ]
      },
      chartOptions: {
        responsive: false,
        maintainAspectRatio: false,
      }
    };
  },
  mounted() {
    this.loadCustom(["hello"], [33])
  },
  methods: {
    loadCustom(labels = [], data = [], ) {
      this.chartData = {
        labels: labels,
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#163531',
            data: data
          }
        ]
      };
    }
  }
};

</script>
