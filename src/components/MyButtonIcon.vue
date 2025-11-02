<template>
  <div class="button-icon">
    <button class="material-icons">
      {{ iconName }}
    </button>
  </div>
</template>

<script setup>

defineProps({
  iconName: String,
})

</script>

<style scoped>
button {
  border: none;
  cursor: pointer;
  background-color: transparent;
}
</style>