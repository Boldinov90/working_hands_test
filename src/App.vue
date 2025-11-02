<script setup>
import Calendar from './components/Calendar.vue'
</script>

<template>
  <div class="calendar-wrapper">
    <Calendar msg="Calendar"/>
  </div>
</template>

<style scoped>
.calendar-wrapper {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
