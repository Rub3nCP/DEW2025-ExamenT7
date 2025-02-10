<template>
  <div>
    <h2>Datos del país</h2>
    <p v-if="!country">Selecciona un país para ver los datos</p>
    <div v-if="country">
      <p>Población: {{ country.population }}</p>
      <p>PIB: {{ country.pib }}</p>
      <p v-if="country.income">PIB per cápita: <span :style="{ color: country.pib / country.population > country.income ? 'red' : 'black' }">{{ (country.pib / country.population).toFixed(2) }}</span></p>
      <button @click="$emit('delete', country.code)">Eliminar</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    country: Object,
  },
};
</script>
