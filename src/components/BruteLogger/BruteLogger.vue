<script setup>

import BruteLoggerItem from "@/components/BruteLogger/BruteLoggerItem.vue";
import BruteLoggerTitle from "@/components/BruteLogger/BruteLoggerTitle.vue";
</script>

<template>
  <div class="be-logger">
    <div class="title">BruteExpose</div>
    <div class="description">
      The implemented system continuously monitors and promptly reports any unauthorized access attempts on my server. Since no data or services are hosted on the server, neither myself nor any other individuals are at risk.
      <br><br>
      Repo: <a href="https://github.com/chomnr/BruteExpose">https://github.com/chomnr/BruteExpose</a>
      <br><br>
      You can view the analytics <a href="#">here</a>.
    </div>
    <div class="log-table">
      <BruteLoggerTitle/>
      <BruteLoggerItem>
        <template #country>US</template>
        <template #username>root</template>
        <template #password>rootin12#!</template>
        <template #source>72.0.31.34.1</template>
        <template #protocol>SSH</template>
      </BruteLoggerItem>
      <BruteLoggerItem>
        <template #country>US</template>
        <template #username>root</template>
        <template #password>rootin12#!</template>
        <template #source>72.0.31.34.1</template>
        <template #protocol>SSH</template>
      </BruteLoggerItem>
    </div>
  </div>
</template>


<style scoped>
 .be-logger {
   display: flex;
   flex-direction: column;
   padding: 5px;
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

 .log-entry {
   display: flex;
   justify-content: center;
 }

 .log-entry.header {
   display: flex;
 }

 .col {
   flex: 1;
   font-size: 0.7rem;
   width: 100px;
   min-width: 50px;
   text-align: center;
 }

 .col.status {
   font-family: var(--logger-status-font);
   min-width: 10px;
 }

 .col.status.success {
   background: var(--logger-status-success);
 }

 .col.status.failed {
   background: var(--logger-status-blocked);
 }

 @media (min-width: 1024px) {
   .be-logger {
     max-width: 700px;
     padding: 0;
   }
 }


 /*
 .be-logger .log-block {
   display: table;
   flex-direction: column;
   gap: 3px;
   width: 100%;
 }

 .be-logger .log-block .log {
   display: table-row;
 }

 .be-logger .log-block .log ul.title {
   margin: 0;
   padding: 0;
   display: table-cell;
 }

 .be-logger .log-block .log ul {
   display: flex;
   align-items: center;
   list-style: none;
   padding: 0;
 }

 .be-logger .log-block .log li.data {
   padding: 5px 5px;
   font-size: 0.8rem;
   background: var(--color-background);
 }

 .be-logger .log-block .status {
   font-family: var(--logger-status-font);
   font-size: 0.6rem;
   padding: 0.3rem 4px;
   text-align: center;
   width: 50px;
 }

 .be-logger .log-block .log .status.success {
   background: var(--logger-status-success);
 }

 .be-logger .log-block .log .status.failed {
   background: var(--logger-status-blocked);
 }
  */
</style>
<!--
<ul class="logger-list">
 <BruteLoggerItem>
   <template #region>CN</template>
   <template #username>root</template>
   <template #password>root123!</template>
   <template #hostname>123.13.21.433</template>
   <template #protocol>SSH</template>
 </BruteLoggerItem>
 <BruteLoggerItem>
   <template #region>CN</template>
   <template #username>root</template>
   <template #password>root123!</template>
   <template #hostname>123.13.21.433</template>
   <template #protocol>SSH</template>
 </BruteLoggerItem>
</ul>
-->