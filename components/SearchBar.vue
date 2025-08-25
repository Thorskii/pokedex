<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
    :placeholder="placeholder"
    class="search-input"
  />
</template>

<script setup lang="ts">
defineProps<{
  modelValue: string
  placeholder?: string
}>()
defineEmits(['update:modelValue'])
</script>

<style scoped>
.search-input {
  padding: 0.5rem;
  width: 100%;
  font-size: 1rem;
  margin-bottom: 1rem;
}
</style>
