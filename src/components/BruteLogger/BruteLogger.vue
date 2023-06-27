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
   <!-- <BruteLoggerChart ref="bar" width="150" height="150" /> -->
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
import BruteLoggerChart from "@/components/BruteLogger/BruteLoggerChart.vue";
import {useWebSocket} from "@vueuse/core";
import {reactive, ref} from "vue";

// Websocket Configuration.
const SOCKET_ADDRESS = ref("ws://localhost:8080");
const CAN_RECONNECT = true;

async function connectToWebSocket() {
  return new Promise(function (resolve, reject) {
    useWebSocket(SOCKET_ADDRESS, {
      autoReconnect: CAN_RECONNECT,
      onConnected() {
        reactivity.isConnected = true;
      },
      onDisconnected() {
        reactivity.isConnected = false;
        reject()
      },
      onError() {
        reactivity.isConnected = false;
        reject();
      },
      onMessage(ws, event){
        populate(event.data);
        update();
        resolve();
      }
    });
  })
}

// Reactivity
let reactivity = reactive({
  attempts: [],
  isConnected: false
})

// Interpreting/Manipulating data from Websocket.

let logs;
let metrics;

function populate(raw_data) {
  const non_raw_data = parse(raw_data);
  let a = non_raw_data["mergeA"] // logs
  let b = non_raw_data["mergeB"]; // metrics
  if (a === undefined && b === undefined) {
    // Checks if the data did not use the BruteExpose function.
    // if mergeA(a) && mergeB(b) are both undefined. That means
    // the merge function was not used therefore we should be
    // expecting a SINGLE DATA ROW instead of a collection.
    logs = non_raw_data;
    return [non_raw_data]
  }
  if (b !== undefined) {
    metrics = b;
  }
  logs = a;
  return [a, b]
}

function update() {
  if (logs !== undefined) {
    if (logs.length > 1) {
      logs.forEach((log) => {
        reactivity.attempts.push(log)
      })
      return true;
    }
    updateSingleLog();
  }
}

function updateSingleLog(){
  if (logs !== undefined && !Array.isArray(logs) ) {
    reactivity.attempts.push(logs)
    reactivity.attempts.unshift(logs)
    reactivity.attempts.pop();
  }
}

function parse(data){
  let json;
  try {
    json = JSON.parse((JSON.parse(data)));
  } catch (error) {
    json = JSON.parse(data);
  }
  return json;
}



export default {
  name: 'BruteLogger',
  components: {BruteLoggerChart},
  async mounted() {
    await connectToWebSocket();
  }
}

/* Charts */
//import {onMounted, reactive, ref} from "vue";
//import BruteLoggerChart from "@/components/BruteLogger/BruteLoggerChart.vue";

//import {Bar, Chart} from 'vue-chartjs'
//import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
//import bruteLoggerChart from "@/components/BruteLogger/BruteLoggerChart.vue";

/* Charts end here. */

/*
WORKING STARTS HERE

const SOCKET_ADDRESS = ref("ws://localhost:8080");

let reactivity = reactive({
  attempts: [],
  isConnected: false
})

let chart = {
  labels: null,
  datasets: [
    {
      label: 'Most Attacks By Country',
      backgroundColor: '#D61531',
      data: [10, 30, 40, 50]
    }
  ]
}

// Managing reactive log DOM
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

// json hedlpers
function hackyParse(data){
  let json;
  try {
    json = JSON.parse((JSON.parse(data)));
  } catch (error) {
    json = JSON.parse(data);
  }
  return json;
}


// Logging
let metrics;
function addLogs(log_data) {
  let json = hackyParse(log_data)
  let logs = json["mergeA"]
  metrics = json["mergeB"]

  chart.labels = getMetricKeys(metrics)


  // Checks whether the sent request is an actual LIST OF LOGS
  if (logs !== undefined) {
    if (logs.length > 1) {
      logs.forEach((log) => {
        reactivity.attempts.push(log)
      })
      return true;
    }
  }

  reactivity.attempts.push(json)
  reactivity.attempts.unshift(json)
  reactivity.attempts.pop();
}

function getMetricKeys(metrics) {
  let map = Object.entries(metrics).keys();
  return Array.from(map.keys());
}

export default {
  name: 'App',
  components: { BruteLoggerChart },
  mounted() {
    //this.$refs.bar.loadCustom(["US", "UK", "CN"], [3313, 13232]);

  }
}

WORKING ENDS HERE.
*/

/*
function loadMetricLabels(json) {
  let map = Object.entries(json);
  reactivity.barChartLabels = ["US", "TEST"]
}
 */

/*
function loadMetricLabels(json) {
  let map = Object.entries(json);
  reactivity.barChartLabels = ["US", "UK", "FLANDER"];
}
 */

/* Analytics */
/*
let currentLabels;
function setMetricLabels(json) {
  if (json !== undefined) {
    let map = Object.entries(json);
    let test = Array.from(map.keys());
    console.log(test)
    currentLabels = Array.from(map.keys());
  } else {
    currentLabels = ["US"]
  }
}
 */
/*
export default {
  name: 'BarChart',
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
*/

/* Charts */
/*
export default {
  name: 'BarChart',
  components: { Bar },
  data() {
    return {
      barChartData: {
        labels: [],
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#163531',
            data: [10000, 5000, 10]
          }
        ]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
      }
    }
  },
}
*/
/*
chartData: {
        labels: [],
        datasets: [
          {
            label: 'Most Attacks By Country',
            backgroundColor: '#163531',
            data: [10000, 5000, 10]
          }
        ]
      }
 */
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