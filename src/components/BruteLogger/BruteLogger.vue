<script setup>

import BruteLoggerItem from "@/components/BruteLogger/BruteLoggerItem.vue";
import BruteLoggerTitle from "@/components/BruteLogger/BruteLoggerTitle.vue";
</script>

<template>
  <div class="title">BruteExpose</div>
  <div class="description">
      <div class="websocket-status failed" v-if="!reactivity.isConnected">STATUS: <b>NOT CONNECTED</b></div>
      <div class="websocket-status" v-else>STATUS: <b>CONNECTED</b></div>
    <br>
    This website continuously monitors and promptly reports any unauthorized access attempts on my server using BruteExpose. Since no data or services are hosted on the server, neither myself nor any other individuals are at risk.

    <br><br>
    Repo: <a href="https://github.com/chomnr/BruteExpose">https://github.com/chomnr/BruteExpose</a>
    <br>
  </div>
  <div class="analytics">
    <Bar :data="chartData" :options="options"  width="150" height="150" />
  </div>
  <div class="be-logger">
    <BruteLoggerTitle/>
    <div id="brute-log" class="log-table">
      <brute-logger-item v-for="attempt in reactivity.attempts">
        <template #country>{{ attempt["country"] }}</template>
        <template #username>{{ attempt["username"] }}</template>
        <template #password>{{ attempt["password"] }}</template>
        <template #source>{{ attempt["hostname"] }}</template>
        <template #protocol>{{ attempt["protocol"] }}</template>
      </brute-logger-item>
    </div>
  </div>
</template>

<script>
import {reactive, ref} from "vue";
import { useWebSocket } from '@vueuse/core'

/* Charts */
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
/* Charts end here. */

const SOCKET_ADDRESS = ref("ws://localhost:8080");

let reactivity = reactive({ attempts: [], isConnected: false })

/* Managing reactive log DOM. */
useWebSocket(SOCKET_ADDRESS, {
  autoReconnect: true,
  onConnected() {
    reactivity.isConnected = true
  },
  onDisconnected() {
    reactivity.isConnected = false;
  },
  onMessage(ws, event) {
    addLogs(event.data)
  }
})

function addLogs(log_data) {
  const json = JSON.parse(log_data);
  if (json.length > 1) {
    json.forEach((log) => {
      reactivity.attempts.push(log)
    })
    return true;
  }

  reactivity.attempts.push(json)
  reactivity.attempts.unshift(json)
  reactivity.attempts.pop();
}

export default {
  name: 'BarChart',
  components: { Bar },
  data() {
    return {
      chartData: {
        labels: [ 'US', 'UK', 'CN', "IR", "AF", "KR", "PG", "ZE", "DA", "ND"],
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#163531',
            data: [10000, 5000, 300]
          }
        ]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
      }
    }
  }
}
</script>

<style scoped>
 .be-logger {
   display: flex;
   flex-direction: column;
   margin-bottom: 20px;
 }

 #brute-log {
   animation: fadeIn 3s;
 }

 @keyframes fadeIn {
   0% { opacity: 0; }
   100% { opacity: 1; }
 }

 .title {
   font-size: 3rem;
 }

 .description {
   border-left: 2px solid var(--logger-border-color);
   font-size: 0.75rem;
   padding-left: 0.5rem;
 }

 .be-logger .log-table {
   display: flex;
   flex-direction: column;
   justify-content: space-evenly;
   margin-top: 10px;
   gap: 2px;
 }

 .websocket-status {
   font-size: var(--logger-col-font-size);
   background: var(--be-c-green);
   width: calc(var(--logger-col-width) + 0.9rem);
   padding: 0 5px;
   font-family: monospace;
 }

 .websocket-status.failed {
   background: var(--be-c-red);
   width: calc(var(--logger-col-width) + 2.4rem);
 }

 /* Analytics */
 .analytics {
   display: flex;
   flex-direction: column;
   justify-content: center;
   align-items: center;
   gap: 10px;
 }

 @media (min-width: 1024px) {
   .be-logger {
     max-width: 700px;
     padding: 0;
   }

   .description {
     max-width: 700px;
   }

   .analytics {
     display: flex;
     flex-direction: row;
     justify-content: center;
     max-width: 700px;
     gap: 10px;
   }
 }
</style>