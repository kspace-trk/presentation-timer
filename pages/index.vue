<template>
  <div class="main-wrapper">
    <timer />
  </div>
</template>

<script>
import timer from '@/components/timer.vue'
export default {
  components: {
    timer,
  },
}
</script>

<style>
.main-wrapper {
  width: 100%;
  height: 100vh;
  display: grid;
  place-items: center;
}
</style>
