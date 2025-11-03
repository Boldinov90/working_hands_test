<template>
  <div class="calendar">
    <div class="calendar__header">
      <MyButtonIcon icon-name="arrow_left" @click="prevMonth" />
      {{ currentMonthAndYear?.month?.name }} {{ currentMonthAndYear?.year }}
      <MyButtonIcon icon-name="arrow_right" @click="nextMonth" />
    </div>
    <div class="calendar__weekdays">
      <div 
        v-for="weekDay in (lang === 'Ru') ? weekDaysRu : weekDaysEn"
        :key="weekDay"
        class="weekday"
      >
        {{ weekDay }}
      </div>
    </div>
    <div class="calendar__days-grid">
      <div
        v-for="day in days"
        :key="day"
        :class="['days-grid__day', {'today': day.isToday || day.isSelected}]"
        @click="onDayClicked(day)"
      >
        {{ day.number }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed, watch } from 'vue'
import MyButtonIcon from './MyButtonIcon.vue';

// Props section
const props = defineProps({
  msg: {
    type: String,
    default: 'Text'
  },
  lang: {
    type: String,
    default: 'En'
  },
  importDate: {
    type: String,
    default: null
  }
})

//Emit section
const emit = defineEmits(['clicked-day'])
const onDayClicked = (event) => {
  emit('clicked-day', event)

}

// Data section
const days = ref([]);
const currentMonthAndYear = ref(null);
const selectedDate = ref(null);

const weekDaysEn = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
const weekDaysRu = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'];
const monthListEn = ['Jen', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
const monthListRu = ['Янв', 'Фев', 'Мар', 'Апр', 'Май', 'Июнь', 'Июль', 'Авг', 'Сен', 'Окт', 'Ноя', 'Дек'];

// Computed section
const daysCount = computed(() => {
  return new Date(Number(currentMonthAndYear.value?.year), Number(currentMonthAndYear.value?.month?.number), 0).getDate();
})
const parseImportDate = () => {
  if (!props.importDate) return new Date();
  try {
    const date = new Date(props.importDate)
    return isNaN(date.getTime()) ? new Date() : date;
  } catch {
    return new Date();
  }
}

// Methods section
const updateSelectDate = () => {
  if (!props.importDate) {
    selectedDate.value = null;
    return;
  }
  try {
    const date = new Date(props.importDate)
    if (!isNaN(date.getTime())) {
      selectedDate.value = {
        day: date.getDate(),
        month: date.getMonth() + 1,
        year: date.getFullYear()
      };
    }
  } catch {
    selectedDate.value = null;
  }
}

const getCurrentMonthAndYear = () => {
  const currentDate = parseImportDate();
  currentMonthAndYear.value = {
    month: {
      number: currentDate.getMonth() + 1,
      name: (props.lang === 'Ru') ? monthListRu[currentDate.getMonth()] : monthListEn[currentDate.getMonth()]
    },
    year: currentDate.getFullYear()
  }
}

const prevMonth = () => {
  if (currentMonthAndYear.value?.month.number === 1) {
    currentMonthAndYear.value.year--
    currentMonthAndYear.value.month.number = 12;
    currentMonthAndYear.value.month.name = (props.lang === 'Ru') ? monthListRu[currentMonthAndYear.value.month.number - 1] : monthListEn[currentMonthAndYear.value.month.number - 1]
  } else {
    currentMonthAndYear.value.month.number--
    currentMonthAndYear.value.month.name = (props.lang === 'Ru') ? monthListRu[currentMonthAndYear.value.month.number - 1] : monthListEn[currentMonthAndYear.value.month.number - 1]
  }
  initDays();
}
const nextMonth = () => {
  if (currentMonthAndYear.value?.month.number === 12) {
    currentMonthAndYear.value.year++
    currentMonthAndYear.value.month.number = 1;
    currentMonthAndYear.value.month.name = (props.lang === 'Ru') ? monthListRu[currentMonthAndYear.value.month.number - 1] : monthListEn[currentMonthAndYear.value.month.number - 1]
  } else {
    currentMonthAndYear.value.month.number++
    currentMonthAndYear.value.month.name = (props.lang === 'Ru') ? monthListRu[currentMonthAndYear.value.month.number - 1] : monthListEn[currentMonthAndYear.value.month.number - 1]
  }
  initDays();
}

const initDays = () => {
  const currentMonthDays = [];
  for (let i = 0; i < daysCount.value; i++) {
    const dayNum = i + 1;
    const date = new Date(currentMonthAndYear.value.year, currentMonthAndYear.value.month.number - 1, dayNum, 12, 0, 0, 0);
    const year = date.getFullYear();
    const month = date.getMonth() + 1;
    const isSelected = selectedDate.value 
                    && selectedDate.value.year === year 
                    && selectedDate.value.month === month 
                    && selectedDate.value.day === dayNum;
    currentMonthDays.push({
      id: Number(`${dayNum}${month}${year}`),
      number: dayNum,
      month,
      year,
      isToday: !props.importDate && date.toDateString() === new Date().toDateString(),
      isSelected: isSelected
    })
  }

  days.value = currentMonthDays
}
watch(() => [props.lang], getCurrentMonthAndYear, {deep: true})
watch(() => props.importDate, () => {
  updateSelectDate();
  getCurrentMonthAndYear();
  initDays();
})
onMounted(() => {
  updateSelectDate();
  getCurrentMonthAndYear();
  initDays();
});

</script>

<style scoped>
.calendar {
  width: 300px;
  height: 300px;
  border: 1px solid rgb(192, 191, 191);
  padding: 5px;
}
.calendar__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.calendar__weekdays{
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  margin-bottom: 10px;
}
.weekday {
  display: flex;
  align-items: center;
  justify-content: center;
}
.calendar__days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-auto-rows: 44px;
  height: calc(100% - 65px);
}
.days-grid__day {
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.today {
  border: 1px solid rgb(103, 228, 212);
}
</style>
