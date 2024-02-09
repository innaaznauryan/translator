<template>
  <button>
    <slot/>
  </button>
</template>

<script setup>

</script>

<style scoped>
button {
  cursor: pointer;
  padding: 10px;
  background-color: cadetblue;
  color: white;
  border: 0;
  border-radius: 5px;
  user-select: none;
}
</style>