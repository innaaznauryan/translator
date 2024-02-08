<template>
  <header>
    <h1>{{ heading[language] }}</h1>
    <div class="control">
      <a v-for="section in sections" :href="'#' + section.id">
        <BaseButton>
          {{ section.title[language] }}
        </BaseButton>
      </a>
      <div class="profileContainer" @click="toggleProfile">
        <img v-if="imgMode" :src="user.avatar" alt="">
        <div v-else class="profile">{{ user.first_name[0] + user.last_name[0] }}</div>
      </div>
      <BaseSelect/>
    </div>
  </header>
</template>

<script setup>
import {language} from "~/composable/language.js"
import {sections} from "~/composable/sections.js"
import {heading} from "~/composable/heading.js"

const response = await useFetch("https://reqres.in/api/users/2")
const user = response.data.value.data
const imgMode = ref(false)

const toggleProfile = () => {
  imgMode.value = !imgMode.value
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px 0;
}

h1 {
  color: cadetblue;
  font-size: 26px;
}

.control {
  display: flex;
  gap: 20px;
}

.profileContainer {
  cursor: pointer;
  width: 40px;
  height: 40px;
  overflow: hidden;
  border-radius: 50%;
  border: 1px solid gray;
  user-select: none;
}

.profileContainer * {
  width: 100%;
  height: 100%;
}

.profile {
  display: grid;
  place-content: center;
  font-weight: 600;
}

img {
  object-fit: cover;
}
</style>