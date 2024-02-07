<template v-if="data">
  <BaseHeader :title/>
  <BaseSection
      v-for="section in data"
      :id="section.id"
      :key="section.id">
    <template #header>
      <h2 @click="handleClick(section.id)">{{ section.title[language] }}</h2>
    </template>
    <template #default>
      <div class="section">
        <div
            class="paragraph"
            v-for="paragraph in section.paragraphs"
            :key="paragraph.id"
            :id="paragraph.id"
            @click="handleClick(paragraph.id)">
          <teleport :to="'#' + paragraph.id">
            <form
                @submit.prevent="handleSubmit(section.id, paragraph)">
              <textarea v-model="paragraph[language]"></textarea>
              <BaseButton type="submit">Save</BaseButton>
              <BaseButton type="button" @click="handleCancel(paragraph.id)">Cancel</BaseButton>
            </form>
          </teleport>
          <teleport :to="'#' + paragraph.id">
            <p>{{ paragraph[language] }}</p>
          </teleport>
        </div>
      </div>
    </template>
  </BaseSection>
</template>

<script setup>
// const response = await useFetch("https://reqres.in/api/resource")
// console.log(response.data.value.data)

import {sections} from "~/composable/sections.js"
import {language} from "~/composable/language.js"

const data = ref(null)

if (process.client) {
  if (localStorage.sections) {
    data.value = JSON.parse(localStorage.sections)
  } else {
    data.value = sections
    localStorage.setItem("sections", JSON.stringify(sections))
  }
}

const titleLanguages = {
  english: "My awesome translator",
  korean: "나의 멋진 번역가"
}
const title = computed(() => titleLanguages[language.value])
const editMode = ref(false)
const elementToEdit = ref(null)

const handleClick = (id) => {
  editMode.value = true
  elementToEdit.value = id
}
// const handleBlur = () => {
//   editMode.value = false
//   elementToEdit.value = null
// }

const handleSubmit = (sectionId, p) => {
  const storedSections = JSON.parse(localStorage.sections) || []
  console.log(p[language.value])
  const updatedParagraph = {...p}
  const updatedSections = storedSections.map(section => section.id === sectionId ? {...section, paragraphs: section.paragraphs.map(paragraph => paragraph.id === p.id ? updatedParagraph : paragraph)} : section)

  console.log(updatedSections)
  // localStorage.setItem("sections", JSON.stringify(updatedSections))
  // data.value = updatedSections
}

const handleCancel = (id) => {
  editMode.value = false
  elementToEdit.value = null
  console.log(id)
  // console.log(editMode.value)
  // console.log(elementToEdit.value)
  // console.log(isEditing())
}

useHead({
  title
})
</script>

<style scoped>
.paragraph {
  margin: 20px 0;
}

textarea {
  display: block;
  width: 100%;
  padding: 5px;
}
</style>
