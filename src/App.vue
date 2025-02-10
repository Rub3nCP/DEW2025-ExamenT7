<script>
import NameInput from '../src/components/NameInput.vue';
import CountryCodes from '../src/components/CountryCodes.vue';
import SelectedCountries from '../src/components/SelectedCountries.vue';
import CountryData from '../src/components/CountryData.vue';
import ChartOptions from '../src/components/CharOptions.vue';
import ChartDisplay from '../src/components/ChartDisplay.vue';

export default {
  components: {
    NameInput,
    CountryCodes,
    SelectedCountries,
    CountryData,
    ChartOptions,
    ChartDisplay,
  },
  data() {
    return {
      countryNames: [],
      chartData: null,
    };
  },
  created() {
    this.chartData = {}; 
  }
};
</script>

<template>
  <div class="parent">
    <div class="header">
      <h1>Datos mundiales</h1>
    </div>
    <div class="name">
      <NameInput />
    </div>
    <div class="div-codes">
      <CountryCodes />
    </div>
    <div class="selected">
      <SelectedCountries :countryNames="countryNames" />
    </div>
    <div class="country-data">
      <CountryData />
      <ChartDisplay :chartData="chartData" />
    </div>
    <div class="options">
      <ChartOptions />
    </div>
    <div class="chart">
      <ChartDisplay />
    </div>
  </div>
</template>

<style scoped>
.parent {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: auto repeat(3, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  height: 100vh;
  margin: -2rem;
}

.header {
  grid-area: 1 / 1 / 2 / 6;
  background-color: aquamarine;
}

.header h1 {
  margin: 2px;
}

.body {
  color: black;
}

.div-codes {
  grid-area: 2 / 1 / 6 / 2;
  background-color: azure;
  overflow-y: auto;
}

.name {
  grid-area: 2 / 2 / 3 / 3;
  background-color: antiquewhite;
}

.selected {
  grid-area: 2 / 5 / 4 / 6;
  background-color: lightyellow;
  overflow-y: auto;
}

.country-data {
  grid-area: 2 / 3 / 3 / 5;
  background-color: lightseagreen;
}

.options {
  grid-area: 3 / 2 / 4 / 5;
  background-color: bisque;
}

.chart {
  grid-area: 4 / 2 / 5 / 6;
  background-color: aqua;
}

ul li {
  text-align: left;
  font-size: 0.9rem;
}

ul li:hover {
  font-weight: bold;
  cursor: pointer
}

ul li button {
  font-size: 0.6rem;
  margin: 2px;
}

h2 {
  font-size: 1.2rem;
}
</style>

