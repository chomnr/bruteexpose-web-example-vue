<script setup>

import BruteLoggerItem from "@/components/BruteLogger/BruteLoggerItem.vue";
import BruteLoggerTitle from "@/components/BruteLogger/BruteLoggerTitle.vue";
</script>

<template>
  <div class="be-logger">
    <div class="log-block">
      <div class="log failed">
        <ul>
          <li class="status">FAILED</li>
          <li class="data">CN &nbsp;</li>
          <li class="data">123.45.132.31</li>
        </ul>
      </div>
    </div>
    <!--todo: title, body-->
  </div>
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
</template>

<style scoped>
 .be-logger {
   /*border-spacing: var(--logger-item-spacing);*/
 }

 .be-logger .log-block {
   display: flex;
   flex-direction: column;
 }

 .be-logger .log-block .log {
   display: flex;
 }

 .be-logger .log-block .log ul {
   display: flex;
   align-items: center;
   list-style: none;
 }

 .be-logger .log-block .log li.data {
   padding-top: 5px;
   padding-bottom: 5px;
   font-size: 0.8rem;
   background: var(--color-background);
 }

 .be-logger .log-block .status {
   font-family: var(--logger-status-font);
   font-size: 0.6rem;
   padding: 0.3rem;
 }

 .be-logger .log-block .log.success {
   background: var(--logger-status-success);
   border-bottom: 1px solid var(--logger-status-success);
 }

 .be-logger .log-block .log.failed {
   background: var(--logger-status-blocked);
   border-bottom: 1px solid var(--logger-status-blocked);
 }
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