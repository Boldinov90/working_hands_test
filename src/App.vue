<template>
  <div class="calendar-wrapper">
    <label>Для демонстрации передачи<br />даты в календарь</label>
    <input v-model="clickedDay" class="input" type="date">
    <div class="select-lang-buttons">
      <button 
        :class="{'active-button': lang === 'En'}"
        @click="lang = 'En'"
      >
        En
      </button>
      <button
        :class="{'active-button': lang === 'Ru'}"
        @click="lang = 'Ru'"
      >
        Ru
      </button>
    </div>
    <Calendar 
      :lang="lang"
      :import-date="clickedDay"
      @clicked-day="onClickedDay"
    />
  </div>
</template>

<script setup>
import Calendar from './components/Calendar.vue'
import {ref} from 'vue'

const lang = ref('En')
const clickedDay = ref(null)

const onClickedDay = (event) => {
  clickedDay.value = new Date(event.year, event.month - 1, event.number + 1).toISOString().slice(0, 10)
  console.log('clicked-day: ', clickedDay.value);
}
</script>

<style scoped>
.calendar-wrapper {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.calendar-wrapper label{
  text-align: center;
  margin-bottom: 10px;
  color: rgb(108, 108, 108);
}
.input{
  width: 300px;
  height: 20px;
  margin-bottom: 10px;
}
.select-lang-buttons {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}
.select-lang-buttons button {
  border: 1px solid gray;
  border-radius: 2px;
  transform: scale(1.2);
  background-color: #EFEFEF;
}
.active-button {
  background-color: #67ace4 !important; 
  transform: scale(1) !important;
}
</style>
