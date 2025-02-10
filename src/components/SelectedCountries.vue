<template>
  <div>
    <h2>Países seleccionados</h2>
    <ul>
      <li v-for="country in countryNames" :key="country.code">
        {{ country.name }} <button @click="removeCountry(country.code)">Desmarcar</button>
      </li>
    </ul>
    <p v-if="countryNames.length === 0">Debes seleccionar algún país en el panel de códigos</p>
  </div>
</template>

<script>
export default {
  props: ['countryNames'],
  methods: {
    removeCountry(countryCode) {
      this.$emit('remove-country', countryCode);
    },
  },
};
</script>
