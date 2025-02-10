<template>
  <div class="country-data">
    <h2>{{ country.name }}</h2>
    <p>PIB: {{ country.gdp }}</p>
    <p>Población: {{ country.population }}</p>
    <p>PIB por habitante: <span :style="{ color: country.gdpPerCapita > country.medianSalary ? 'red' : 'black' }">{{ country.gdpPerCapita }}</span></p>
    <p v-if="error">Error: {{ error }}</p>
  </div>
</template>

<script>
export default {
  props: {
    country: Object,
    error: String
  },
  computed: {
    pibPerCapita() {
      return this.country.gdp / this.country.population;
    }
  }
}
</script>