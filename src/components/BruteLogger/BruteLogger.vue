<script setup>

import BruteLoggerItem from "@/components/BruteLogger/BruteLoggerItem.vue";
import BruteLoggerTitle from "@/components/BruteLogger/BruteLoggerTitle.vue";
</script>

<template>
  <div class="be-logger">
    <div class="log-table">
        <div class="log-entry header">
          <div class="col"></div>
          <div class="col">Country</div>
          <div class="col">Username</div>
          <div class="col">Password</div>
          <div class="col">Source</div>
          <div class="col">Protocol</div>
        </div>
      <div class="log-entry">
        <div class="col status failed">FAILED</div>
        <div class="col">US</div>
        <div class="col">Hendrickdssd</div>
        <div class="col">pinhead</div>
        <div class="col">72.0.31.34.1</div>
        <div class="col">SSH</div>
      </div>
      <div class="log-entry">
        <div class="col status failed">FAILED</div>
        <div class="col">US</div>
        <div class="col">Hendrickdssd</div>
        <div class="col">pinhead</div>
        <div class="col">72.0.31.34.1</div>
        <div class="col">SSH</div>
      </div>
      <div class="log-entry">
        <div class="col status success">SUCCESS</div>
        <div class="col">US</div>
        <div class="col">Hendrickdssd</div>
        <div class="col">pinhead</div>
        <div class="col">72.0.31.34.1</div>
        <div class="col">SSH</div>
      </div>
    </div>
  </div>
    <!--
    <div class="log-block">
        <div class="log">
          <div class="data">Region</div>
          <div class="data">Username</div>
          <div class="data">Password</div>
          <div class="data">Source</div>
          <div class="data">Protocol</div>
        </div>
    </div>
    -->
    <!--
    <div class="log-block">
      <div class="log title">
        <ul>
          <li class="data">Region</li>
          <li class="data">Username</li>
          <li class="data">Password</li>
          <li class="data">Source</li>
          <li class="data">Protocol</li>
        </ul>
      </div>
      <div class="log">
        <ul>
          <li class="status failed">FAILED</li>
          <li class="data">US</li>
          <li class="data">root</li>
          <li class="data">rootwizard123!</li>
          <li class="data">12.1.434.31</li>
          <li class="data">SSH</li>
        </ul>
      </div>
      <div class="log">
        <ul>
          <li class="status failed">FAILED</li>
          <li class="data">IR</li>
          <li class="data">admin</li>
          <li class="data">adminplesk</li>
          <li class="data">13.451.132.31</li>
          <li class="data">SSH</li>
        </ul>
      </div>
    </div>
    -->
  <!--
  <div class="log-entry">
          <ul>
            <li>CN</li>
            <li>SSH</li>
          </ul>
        </div>

  <table class="logger-list">
    <BruteLoggerTitle></BruteLoggerTitle>
    <BruteLoggerItem>
      <template #region>CN</template>
      <template #username>root</template>
      <template #password>root123!</template>
      <template #hostname>123.13.21.433</template>
      <template #protocol>SSH</template>
    </BruteLoggerItem>

  </table>
  -->
  <!--todo: title, body-->

</template>


<style scoped>
 .be-logger {
   display: flex;
   flex-direction: column;
   /*border-spacing: var(--logger-item-spacing);*/
 }

 .be-logger .log-table {
   display: flex;
   flex-direction: column;
   justify-content: space-evenly;
   gap: 2px;
 }

 .log-entry { display: flex; }
 .log-entry.header { display: flex; }
 .log-entry.header > * { font-size: 0.95rem }

 .col {
    flex: 1;
    font-size: 0.7rem;
    min-width: 100px;
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