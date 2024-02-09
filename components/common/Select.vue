<template>
  <select @change="handleChange">
    <option value="english">English</option>
    <option value="korean">Korean</option>
  </select>
</template>

<script setup>
import {language} from "~/composable/language.js"

const handleChange = (e) => {
  language.value = e.target.value
}
</script>

<style scoped>
select {
  cursor: pointer;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid gray;
}
</style>