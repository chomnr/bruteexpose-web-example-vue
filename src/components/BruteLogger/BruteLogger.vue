<script setup>

import BruteLoggerItem from "@/components/BruteLogger/BruteLoggerItem.vue";
import BruteLoggerTitle from "@/components/BruteLogger/BruteLoggerTitle.vue";
</script>

<template>

  <div class="title">BruteExpose</div>
  <div class="description">
    This website continuously monitors and promptly reports any unauthorized access attempts on my server using BruteExpose. Since no data or services are hosted on the server, neither myself nor any other individuals are at risk.

    <br><br>
    Repo: <a href="https://github.com/chomnr/BruteExpose">https://github.com/chomnr/BruteExpose</a>
    <br><br>
    You can view the analytics <a href="#">here</a>.
  </div>
  <div class="be-logger">
    <BruteLoggerTitle/>
    <div id="brute-log" class="log-table">
      <brute-logger-item v-for="attempt in attempts">
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
import {ref, VueElement, watchEffect} from "vue";

const socket = new WebSocket("ws://localhost:8080");

let attempts = ref([]);
let isConnected = false;

socket.addEventListener('message', (event) => {
  const response = event.data;
  try {
    isConnected = true;
    const res_json = JSON.parse(response.toString());
    if (res_json.length > 1) {
      res_json.forEach((log) => {
        attempts.value.push(log);
      })
      return true;
    }
    attempts.value.push(res_json);
    attempts.value.unshift(res_json);
  } catch (error) {
    return false;
  }
})

socket.onclose = function (event) {
  isConnected = false;
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