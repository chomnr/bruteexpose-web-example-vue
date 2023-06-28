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
    <BruteLoggerChart ref="bar" width="500" height="150" />
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
import {reactive, ref, watch} from "vue";

// Websocket Configuration.
const SOCKET_ADDRESS = ref("ws://localhost:8080");
const CAN_RECONNECT = true;

// Vue Reactivity Variables
let reactivity = reactive({
  attempts: [],
  metrics: [],
  isConnected: false,
})

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

// Interpreting/Manipulating data from Websocket.
let logs;
function populate(raw_data) {
  const non_raw_data = parse(raw_data);
  let a = non_raw_data["mergeA"] // logs
  let b = non_raw_data["mergeB"]; // metrics

  if (a === undefined && b === undefined) {
    // Checks if the data did not use the BruteExpose merge function.
    // if mergeA(a) && mergeB(b) are both undefined. That means
    // the merge function was not used therefore we should be
    // expecting a SINGLE DATA ROW instead of a collection.
    logs = ref(non_raw_data).value;
    return [non_raw_data]
  }

  if (b !== undefined) {
    reactivity.metrics = b;
  }
  logs = ref(a).value;
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

function parse(data) {
  let json;
  try {
    json = JSON.parse((JSON.parse(data)));
  } catch (error) {
    json = JSON.parse(data);
  }
  return json;
}

// Chart Helper Functions. Pertains to only (Most Attacks By Country).
function getMetricLabels() {
  return Object.keys(reactivity.metrics);
}

function getMetricValues() {
  return Object.values(reactivity.metrics);
}

export default {
  name: 'BruteLogger',
  components: {BruteLoggerChart},
  async mounted() {
    await connectToWebSocket();

    // Not ideal.
    setInterval(() => {
      this.$refs.bar.loadCustom(getMetricLabels(), getMetricValues())
    }, 700)
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
     gap: 10px;
   }
 }
</style>