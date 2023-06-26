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
import {reactive} from "vue";

const SOCKET_ADDRESS = "ws://localhost:8080";
const RECONNECT_TIME = (5 * 1000);

let reactivity = reactive({ attempts: [], isConnected: false })
let socket;
let reconnect;

connectToSocket();

/* Managing reactive log DOM. */
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
}

/* Managing socket connection */
function connectToSocket(){
  try {
    socket = new WebSocket(SOCKET_ADDRESS)
  } catch (error) {
    reactivity.isConnected = false;
  }

  socket.addEventListener('message', (event) => {
    const resp = event.data;
    addLogs(resp)
  });

  socket.addEventListener('open', (event) => {
    reactivity.isConnected = true;
    clearInterval(reconnect)
  });

  socket.onclose = function (event) {
    reactivity.isConnected = false;
    if (!reactivity.isConnected) {
      reconnect = setInterval(connectToSocket, RECONNECT_TIME)
    }
  }
}
</script>

<style scoped>
 .be-logger {
   display: flex;
   flex-direction: column;
   padding: 5px;
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

 @media (min-width: 1024px) {
   .be-logger {
     max-width: 700px;
     padding: 0;
   }

   .description {
     max-width: 700px;
   }
 }

</style>