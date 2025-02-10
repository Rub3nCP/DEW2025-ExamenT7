<script setup>
const props = defineProps({
  data: Object,
});

defineEmits(["remove-country"]);
</script>

<template>
  <template v-if="data">
    <div class="container">
      <span> <b>Nombre:</b> {{ data.name }} </span>
      <span> <b>Población:</b> {{ data.population }} millones </span>
      <span> <b>PIB:</b> {{ data.pib }} millones </span>
      <span> <b>Área:</b> {{ data.area }} km<sup>2</sup></span>
      <span> <b>Renta:</b> {{ data.income }} </span>
      <span>
        <b>PIB/Habitante</b> {{ (data.pib / data.population).toFixed(2) }}
      </span>
    </div>
    <button @click="$emit('remove-country', data.code)">Eliminar</button>
  </template>

  <div v-else>
    <b>No hay datos que mostrar</b>
  </div>
</template>

<style scoped>
.container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}
</style>
