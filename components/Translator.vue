<template>
  <BaseHeader/>
  <section
      v-for="section in sections"
      :id="section.id"
      :key="section.id">
    <template v-if="editMode && idToEdit === section.id">
      <form
          :id="'form' + section.id"
          @submit.prevent="e => submitTitle(e, section)">
        <input required type="text" name="section" :value="section.title[language]">
      </form>
      <ButtonContainer :id="section.id" :buttonLanguages :handleClick/>
    </template>
    <template v-else>
      <div class="title">
        <h2>{{ section.title[language] }}</h2>
        <IconEdit @click="handleClick(section.id)"/>
      </div>
    </template>
    <div
        class="paragraph"
        v-for="paragraph in section.paragraphs"
        :key="paragraph.id">
      <template v-if="editMode && idToEdit === paragraph.id">
        <form
            :id="'form' + paragraph.id"
            @submit.prevent="e => submitSection(e, section.id, paragraph)">
          <textarea required name="paragraph" :value="paragraph[language]"></textarea>
        </form>
        <ButtonContainer :id="paragraph.id" :buttonLanguages :handleClick/>
      </template>
      <template v-else>
        <p>{{ paragraph[language] }}</p>
        <IconEdit @click="handleClick(paragraph.id)"/>
      </template>
    </div>
  </section>
</template>

<script setup>
if (process.client) {
  if (localStorage.sections) {
    sections.value = JSON.parse(localStorage.sections)
  } else {
    localStorage.setItem("sections", JSON.stringify(sections.value))
  }
}

import {IconEdit} from '@tabler/icons-vue'
import {sections} from "~/composable/sections.js"
import {language} from "~/composable/language.js"
import {heading} from "~/composable/heading.js"

const buttonLanguages = {
  edit: {english: "Edit", korean: "편집하다"},
  save: {english: "Save", korean: "구하다"},
  cancel: {english: "Cancel", korean: "취소"}
}
const title = computed(() => heading.value[language.value])
const editMode = ref(false)
const idToEdit = ref(null)

const handleClick = (id) => {
  if (editMode.value) {
    if (idToEdit.value === id) {
      editMode.value = false
      idToEdit.value = null
    } else {
      idToEdit.value = id
    }
  } else {
    editMode.value = true
    idToEdit.value = id
  }
}

const submitSection = (e, sectionId, paragraph) => {
  const updatedParagraph = {...paragraph, [language.value]: e.target.paragraph.value}
  const updatedSections = sections.value.map(section => section.id === sectionId ? {
    ...section,
    paragraphs: section.paragraphs.map(elem => elem.id === paragraph.id ? updatedParagraph : elem)
  } : section)
  localStorage.setItem("sections", JSON.stringify(updatedSections))
  sections.value = updatedSections
  editMode.value = false
  idToEdit.value = null
}

const submitTitle = (e, section) => {
  const updatedSection = {...section, title: {...section.title, [language.value]: e.target.section.value}}
  const updatedSections = sections.value.map(elem => elem.id === section.id ? updatedSection : elem)
  localStorage.setItem("sections", JSON.stringify(updatedSections))
  sections.value = updatedSections
  editMode.value = false
  idToEdit.value = null
}

useHead({
  title: title
})
</script>

<style scoped>
.title {
  display: flex;
  align-items: center;
  gap: 10px;
}

h2 {
  font-size: 20px;
  color: cadetblue;
}

.paragraph {
  margin: 20px 0;
  line-height: 1.5;
}

p {
  text-align: justify;
}

input {
  padding: 5px;
  border-radius: 5px;
  border: 1px solid gray;
}

textarea {
  display: block;
  width: 100%;
  padding: 5px 10px;
  border-radius: 5px;
  border: 1px solid gray;
  resize: none;
  line-height: 1.5;
  font-family: sans-serif;
  text-align: justify;
}
</style>
