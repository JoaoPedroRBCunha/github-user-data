<script setup>
import { ref } from 'vue';
import {searchHistory} from '../stores/searchHistory'

const emit = defineEmits(['formSubmit', 'update:modelValue']);
const searchInput = ref('')

function handleSubmit(ev) {
  ev.preventDefault();
  searchHistory.users.push(searchInput.value)
  emit('formSubmit', searchInput.value)
}

function showSearchHistory() {
  alert(`Histórico de Pesquisa:\n${searchHistory.users.join('\n')}`)
}
</script>

<template>
  <form @submit="handleSubmit">
    <input
      type="text"
      v-model="searchInput"
      @input = "$emit('update:modelValue', $event.target.value)"
    />
    <button type="submit">Carregar Usuário</button>
    <button type="button" @click="showSearchHistory">Ver Histórico</button>
  </form>
</template>

<style scoped>
input,
button {
  max-width: 20rem;
  padding: .5rem;
}
input {
  background-color: #1c1a1d;
  border: 1px solid #f64348;
  border-radius: .25rem;
  color: #e5e5e5;
  margin: 1rem 1rem 1rem 0;
}
button {
  background-color: #f64348;
  border: unset;
  border-radius: .25rem;
  color: #1c1a1d;
  font-size: 1rem;
  font-weight: 700;
  margin-left: 1rem;
  text-transform: uppercase;
}
button:hover {
  cursor: pointer;
  filter: brightness(0.95);
}
</style>