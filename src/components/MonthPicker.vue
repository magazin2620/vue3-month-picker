<template>
  <div class="month-picker__container" :class="{ dark: isDark }">
    <div class="month-picker__year">
      <button type="button" @click="decrement">&lsaquo;</button>
      <p>{{ year }}</p>
      <button type="button" @click="increment">&rsaquo;</button>
    </div>
    <div class="month-picker">
      <div
        v-for="(month, monthIndex) in monthsByLang"
        :key="month"
        :class="{ selected: currentMonthIndex === monthIndex }"
        class="month-picker__month"
        @click="selectMonth(monthIndex)"
      >
        {{ month }}
      </div>
    </div>
  </div>
</template>

<script setup>
import languages from '../languages';
import { ref, computed, defineProps, defineEmits } from 'vue';

const props = defineProps({
  lang: String,
  isDark: Boolean,
});

const emit = defineEmits(['change-year', 'change-month']);

const year = ref(new Date().getFullYear());
const currentMonthIndex = ref(0);

function increment() {
  year.value++;
  emit('change-year', year.value);
}
function decrement() {
  year.value--;
  emit('change-year', year.value);
}
function selectMonth(index) {
  const isAlreadySelected = currentMonthIndex.value === index;
  if (isAlreadySelected) {
    return;
  }
  currentMonthIndex.value = index;
  const currentMonth = monthsByLang.value[currentMonthIndex.value];
  emit('change-month', currentMonth);
}

const monthsByLang = computed(() => {
  return languages[props.lang];
});
</script>

<style>
.month-picker__container {
  width: 400px;
  position: relative;
  box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.1),
    inset 0 -1px 0 rgba(0, 0, 0, 0.1);
  border-radius: 5px;
}

.month-picker {
  box-sizing: border-box;
  flex: 1;
  width: auto;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  box-shadow: 1px 2px 5px rgba(0, 0, 0, 0.1);
  font-family: sans-serif;
  border-radius: 5px;
  overflow: hidden;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.month-picker__year {
  padding: 0.2rem;
  background-color: #fcfcfc;
  box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.1),
    inset 0 -1px 0 rgba(0, 0, 0, 0.1);
}

.month-picker__year p {
  width: 100%;
  font-weight: 600;
  letter-spacing: 1px;
  font-size: 1.2rem;
  margin: 1rem 0;
}

.month-picker__year div,
.month-picker__year button,
.month-picker__year p {
  text-align: center;
  flex: 1;
}

.month-picker__year button {
  background-color: #f4f4f4;
  position: absolute;
  width: 5rem;
  height: 2.75rem;
  font-size: 2rem;
  border-radius: 5px;
  outline: none;
  border: 0;
  top: 0.5rem;
  border: 1px solid #e8e8e8;
  z-index: 2;
  color: #686868;
  cursor: pointer;
}

.month-picker__year button:hover {
  background-color: rgba(0, 0, 0, 0.025);
}

.month-picker__year button:active {
  background-color: rgba(0, 0, 0, 0.04);
}

.month-picker__year button:first-child {
  left: 10px;
}

.month-picker__year button:last-child {
  right: 10px;
}

.month-picker__month {
  padding: 0.85rem 0.25rem;
  flex-basis: calc(33.333% - 10px);
  padding: 0.75rem 0.25rem;
  cursor: pointer;
  text-align: center;
  border: 1px solid rgba(245, 245, 245, 0.75);
  transition: all 250ms cubic-bezier(0.165, 0.84, 0.44, 1);
  background-color: #fefefe;
  user-select: none;
  position: relative;
}

.month-picker__month:hover::after {
  display: block;
  content: '';
  position: absolute;
  width: 95%;
  height: 95%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  z-index: 10;
}

.month-picker__month.selected {
  background-color: #55b0f2;
  color: #ffffff;
  border-radius: 5px;
  box-shadow: inset 0 0 3px #3490d2, 0px 2px 5px rgba(85, 176, 242, 0.2);
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.25);
}

/* Dark theme */
.dark {
  background-color: #5f5f5f;
}

.dark .month-picker__year {
  background-color: #2f2f30;
}

.dark .month-picker__year p {
  color: #ebebeb;
}

.dark .month-picker__year button {
  background-color: #505050;
  color: #c9c9c9;
  border-color: #1e1e1e;
}

.dark .month-picker__year button:hover {
  background-color: rgba(0, 0, 0, 0.3);
}

.dark .month-picker__year button:active {
  background-color: rgba(0, 0, 0, 0.6);
}

.dark .month-picker__month {
  background-color: #2f2f30;
  border-color: rgba(245, 245, 245, 0.15);
  color: #c9c9c9;
}

.dark .month-picker__month.selected {
  background-color: #505050;
  box-shadow: inset 0 0 3px #505050, 0px 2px 5px #505050;
  color: #ffffff;
  border-color: #1d1b1b;
}

.dark .month-picker__month:hover {
  box-shadow: 0 0 6px rgba(0, 0, 0, 0.8);
  z-index: 10;
}

@media only screen and (max-width: 768px) {
  .month-picker__container {
    width: 100%;
  }
}
</style>
