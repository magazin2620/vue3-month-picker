<template>
  <div>
    <div class="content">
      <h1>vue3-month-picker</h1>

      <h2>Parameters</h2>

      <div class="form__container">
        <label class="form__label">
          isDark
          <input type="checkbox" id="isDark" v-model="checked" />
          <label for="isDark">{{ checked }}</label>
        </label>

        <label class="form__label">
          lang
          <select v-model="selectedLang">
            <option
              v-for="(lang, langKey) in languages"
              :key="langKey"
              :value="langKey"
            >
              {{ langKey }}
            </option>
          </select>
        </label>
      </div>

      <h2>Return value</h2>

      <div>
        <pre>{{ date }}</pre>
      </div>

      <br />
      <br />

      <month-picker
        :lang="selectedLang"
        :isDark="checked"
        @change-year="(year) => (date.year = year)"
        @change-month="(month) => (date.month = month)"
      />

      <br />
      <br />
    </div>
  </div>
</template>

<script setup>
import languages from './languages';
import MonthPicker from './components/MonthPicker.vue';
import { ref } from 'vue';

const selectedLang = ref('en');
const checked = ref(false);
const date = ref({
  year: null,
  month: null,
});
</script>

<style>
:root {
  background-color: #efefef;
}
</style>

<style scoped>
.content {
  max-width: 800px;
  margin: 0 auto;
}

.content h1 {
  margin-bottom: 2em;
}

.form__container {
  padding: 0.25rem 0;
}

.form__label {
  display: flex;
  max-width: 12rem;
  padding: 0.5rem;
  margin-bottom: 1rem;
}

.form__label:hover {
  background-color: #f7f7f7;
}

.form__label input {
  margin-left: 6rem;
}
.form__label label {
  margin-left: 0.3rem;
}

.form__label select {
  margin-left: 7rem;
  width: 5rem;
}

pre {
  font-family: monospace;
  font-size: 1rem;
  font-weight: 600;
  color: #272525;
}
</style>
