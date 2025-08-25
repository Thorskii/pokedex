<template>
  <div class="search-container">
    <!-- Name search input -->
    <input
      type="text"
      :value="modelValue.name"
      @input="$emit('update:modelValue', { ...modelValue, name: $event.target.value })"
      placeholder="Search Pokémon by name..."
      class="search-input"
    />

    <!-- Type dropdown -->
    <select
      :value="modelValue.type"
      @change="$emit('update:modelValue', { ...modelValue, type: $event.target.value })"
      class="search-select"
    >
      <option value="">All types</option>
      <option v-for="t in types" :key="t" :value="t">{{ capitalize(t) }}</option>
    </select>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  modelValue: { name: string; type: string }
  types: string[]
}>()

defineEmits(['update:modelValue'])

const capitalize = (str: string) => str.charAt(0).toUpperCase() + str.slice(1)
</script>

<style scoped>
.search-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.search-input {
  padding: 0.5rem;
  width: 300px;
  max-width: 90%;
  font-size: 1rem;
  border-radius: 25px;
  border: 1px solid #ccc;
  box-sizing: border-box;
}

.search-select {
  padding: 0.5rem;
  font-size: 1rem;
  border-radius: 25px;
  border: 1px solid #ccc;
  min-width: 150px;
  color: gray
}
</style>
