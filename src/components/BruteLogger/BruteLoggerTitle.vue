<script setup>

</script>

<template>
  <div class="log-entry header">
    <div class="col" style="background: var(--color-background) !important"></div>
    <div class="col">Country</div>
    <div class="col">Username</div>
    <div class="col">Password</div>
    <div class="col">Source</div>
    <div class="col">Protocol</div>
  </div>
</template>

<style scoped>
.log-entry {
  position: relative;
  top: 9px;
  display: flex;
  justify-content: center;
}

.log-entry.header {
  display: flex;
  background: var(--be-c-black-light);
}

.col {
  flex: 1;
  font-size: var(--logger-col-font-size);
  width: var(--logger-col-width);
  min-width: var(--logger-col-min-width);
  text-align: var(--logger-col-text-align);
}
</style>