<template>
  <div class="btnContainer">
    <BaseButton
        @click="handleClick(id)">
      {{ buttonLanguages.cancel[language] }}
    </BaseButton>
    <BaseButton
        :form="'form' + id"
        type="submit">
      {{ buttonLanguages.save[language] }}
    </BaseButton>
  </div>
</template>

<script setup>
import {language} from "~/composable/language.js"

defineProps(["id", "buttonLanguages", "handleClick"])
</script>

<style scoped>
.btnContainer {
  display: flex;
  gap: 5px;
  padding: 5px 0;
}
</style>