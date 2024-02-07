<template>
  <header>
    <h1>{{ title }}</h1>
    <BaseButton v-for="section in sections">
      <a :href="'#' + section.id">{{ section.title[language] }}</a>
    </BaseButton>
    <BaseSelect/>
  </header>
</template>

<script setup>
import {sections} from "~/composable/sections.js"
import {language} from "~/composable/language.js"

defineProps(["title"])
</script>

<style scoped>

</style>