<script setup>

</script>

<template>
  <div class="log-entry">
    <div class="col status failed">FAILED</div>
    <div class="col"><slot name="country"></slot></div>
    <div class="col"><slot name="username"></slot></div>
    <div class="col"><slot name="password"></slot></div>
    <div class="col"><slot name="source"></slot></div>
    <div class="col"><slot name="protocol"></slot></div>
  </div>
</template>
<style scoped>

.log-entry {
  display: flex;
  justify-content: center;
}

.log-entry.header {
  display: flex;
}

.col {
  flex: 1;
  font-size: var(--logger-col-font-size);
  width: var(--logger-col-width);
  min-width: var(--logger-col-min-width);
  text-align: var(--logger-col-text-align);
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

</style>