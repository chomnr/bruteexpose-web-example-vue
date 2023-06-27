<template>
  <div>
    <Bar :data="localChartData" :options="localChartOptions" ref="bar" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs';

export default {
  name: 'BruteLoggerChart',
  components: { Bar },
  data() {
    return {
      localChartData: {
        labels: ['lol'],
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#163531',
            data: [10000, 5000, 10]
          }
        ]
      },
      localChartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      }
    };
  },
  mounted() {
    this.loadCustom(["hello"], [33])
  },
  methods: {
    loadCustom(labels = [], data = [], ) {
      this.localChartData = {
        labels: labels,
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#D61531',
            data: data
          }
        ]
      };
    }
  }
};
</script>

<!--
loadDefault() {
      let newArray = [];
      for (let i = 0; i < 3; i++) {
        let randomValue = Math.floor(Math.random() * 10);
        newArray.push(randomValue);
      }
      console.log('NEW DATA');

      this.localChartData = {
        labels: ['test'],
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#D61531',
            data: newArray
          }
        ]
      };
    }
-->



<!--
export default {
  name: 'BruteLoggerChart',
  components: { Bar },
  computed: {
    chartData() { return {
      labels: ['lol'],
      datasets: [
        {
          label: 'Most Attacks By Country',
          backgroundColor: '#163531',
          data: [10000, 5000, 10]
        }
      ]
    }},
    chartOptions() { return {
      responsive: true,
      maintainAspectRatio: false,
    }},
  }
}
 -->