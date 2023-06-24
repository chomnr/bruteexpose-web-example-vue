<script setup>

</script>

<template>
  <tbody>
    <tr class="logger-item">
      <td><slot name="region"></slot></td>
      <td><slot name="username"></slot></td>
      <td><slot name="password"></slot></td>
      <td><slot name="hostname"></slot></td>
      <td><slot name="protocol"></slot></td>
  </tr>
  </tbody>
</template>

<style scoped>

@media (min-width: 1024px) {
  .logger-item {
    font-size: 0.95rem;
  }
  .logger-item .region {}
  .logger-item .username {}
  .logger-item .password {}
  .logger-item .hostname {}
  .logger-item .protocol {}
}
</style>