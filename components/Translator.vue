<template>
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
      <ButtonContainer :id="section.id" :buttonLanguages :toggleEditMode/>
      <span class="error" v-if="inputError && inputErrorId === section.id">{{ inputError }}</span>
    </template>
    <template v-else>
      <div class="sectionTitle">
        <h2>{{ section.title[language] }}</h2>
        <IconEdit @click="toggleEditMode(section.id)"/>
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
          <textarea required name="paragraph" rows="5" :value="paragraph[language]"></textarea>
        </form>
        <ButtonContainer :id="paragraph.id" :buttonLanguages :toggleEditMode/>
        <span class="error" v-if="inputError && inputErrorId === paragraph.id">{{ inputError }}</span>
      </template>
      <template v-else>
        <p>{{ paragraph[language] }}</p>
        <IconEdit @click="toggleEditMode(paragraph.id)"/>
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

const buttonLanguages = {
  edit: {english: "Edit", korean: "편집하다"},
  save: {english: "Save", korean: "구하다"},
  cancel: {english: "Cancel", korean: "취소"}
}
const editMode = ref(false)
const idToEdit = ref(null)
const inputError = ref(null)
const inputErrorId = ref(null)

const toggleEditMode = (id) => {
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
  if (!e.target.paragraph.value.trim()) {
    handleInputError(paragraph.id)
    return
  }
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
  if (!e.target.section.value.trim()) {
    handleInputError(section.id)
    return
  }
  const updatedSection = {...section, title: {...section.title, [language.value]: e.target.section.value}}
  const updatedSections = sections.value.map(elem => elem.id === section.id ? updatedSection : elem)
  localStorage.setItem("sections", JSON.stringify(updatedSections))
  sections.value = updatedSections
  editMode.value = false
  idToEdit.value = null
}

function handleInputError(id) {
  inputError.value = "Please fill out this field"
  inputErrorId.value = id
  setTimeout(() => {
    inputError.value = null
    inputErrorId.value = null
  }, 3000)
}
</script>

<style scoped>
.sectionTitle {
  display: flex;
  align-items: center;
  gap: 10px;
}

h2 {
  font-size: 20px;
  color: cadetblue;
}

.error {
  color: red;
  padding: 0 5px;
}

.paragraph {
  margin: 20px 0;
  line-height: 1.5;
}

svg {
  opacity: 0;
  transition: .3s;
}

.paragraph:hover svg,
.sectionTitle:hover svg {
  opacity: 1;
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
  font-size: 16px;
}
</style>
