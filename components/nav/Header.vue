<template>
  <header>
    <h1>{{ title }}</h1>
    <div class="control">
      <NuxtLink v-for="section in sections" :to="{path: '/', hash: '#' + section.id}">
        <CommonButton>
          {{ section.title[language] }}
        </CommonButton>
      </NuxtLink>
      <div class="profileContainer" @click="toggleProfile">
        <img v-if="imgMode" :src="user.avatar" alt="">
        <div v-else class="profile">{{ user.first_name[0] + user.last_name[0] }}</div>
      </div>
      <CommonSelect/>
    </div>
  </header>
</template>

<script setup>
import {language} from "~/composable/language.js"
import {sections} from "~/composable/sections.js"

const response = await useFetch("https://reqres.in/api/users/2")
const user = response.data.value.data
const imgMode = ref(false)
const headings = {
  english: "My awesome translator",
  korean: "나의 멋진 번역가"
}
const title = computed(() => headings[language.value])

const toggleProfile = () => {
  imgMode.value = !imgMode.value
}

useHead({
  title: title
})
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