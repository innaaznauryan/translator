<template>
  <div class="btnContainer">
    <CommonButton
        @click="toggleEditMode(id)">
      {{ buttonLanguages.cancel[language] }}
    </CommonButton>
    <CommonButton
        :form="'form' + id"
        type="submit">
      {{ buttonLanguages.save[language] }}
    </CommonButton>
  </div>
</template>

<script setup>
import {language} from "~/composable/language.js"

defineProps(["id", "buttonLanguages", "toggleEditMode"])
</script>

<style scoped>
.btnContainer {
  display: inline-flex;
  gap: 5px;
  padding: 5px 0;
}
</style>