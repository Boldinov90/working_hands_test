<template>
  <div class="calendar">
    <div class="calendar__header">
      <MyButtonIcon icon-name="arrow_left" />
      {{ msg }}
      <MyButtonIcon icon-name="arrow_right" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import MyButtonIcon from './MyButtonIcon.vue';

defineProps({
  msg: String,
})


</script>

<style scoped>
.calendar {
  width: 300px;
  height: 300px;
  border: 1px solid rgb(192, 191, 191);
}
.calendar__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
