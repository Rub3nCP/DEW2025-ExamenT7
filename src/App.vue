<script setup>
import { ref, onMounted, computed, reactive } from "vue";
import codes from "./data/codes.json";
import GoogleChart from "./components/GoogleChart.vue";
import CountryData from "./components/CountryData.vue";

const name = ref("");
const selectedCountries = ref([]); 
const selectedCountriesData = reactive({});
const countryNames = ref({});

const lastSelectedCountry = ref(null);

const chartMode = ref("population");

const chartModes = {
  population: "Población",
  pib: "PIB",
  area: "Área",
  income: "Renta",
};

onMounted(async () => {
  try {
    const res = await fetch("http://localhost:3000/api/names");
    countryNames.value = await res.json();
  } catch (error) {
    console.error("Error cargando los nombres de los países:", error);
  }
});

function getCountryName(code) {
  return countryNames.value[code] || code;
}

async function selectCountry(code) {
  if (selectedCountries.value.includes(code)) return; 
  selectedCountries.value.push(code);

  try {
    const res = await fetch(`http://localhost:3000/api/country/${code}`);
    if (!res.ok) {
      throw new Error(`País no encontrado: ${code}`);
    }
    
    lastSelectedCountry.value = await res.json();
    selectedCountriesData[code] = lastSelectedCountry.value; 
  } catch (error) {
    console.error("Error al cargar datos del país:", error);
  }
}

function unselectCountry(code) {
  const index = selectedCountries.value.indexOf(code);
  if (index !== -1) {
    selectedCountries.value.splice(index, 1); 
  }
  delete selectedCountriesData[code];
  if (lastSelectedCountry.value && lastSelectedCountry.value.code === code) {
    lastSelectedCountry.value = null;
  }
}

const sortedSelectedCountries = computed(() => {
  return selectedCountries.value.slice().sort((a, b) => {
    const nameA = getCountryName(a).toUpperCase();
    const nameB = getCountryName(b).toUpperCase();
    return nameA.localeCompare(nameB);
  });
});

const chartCountries = computed(() => {
  const result =  [
    ["país", chartModes[chartMode.value]],
    ...sortedSelectedCountries.value.map((code) => {
      const country = countryNames.value[code] || code;
      const data = selectedCountriesData[code];
      return [country, data ? data[chartMode.value] : 0];
    }),
  ];
  return result;
});
</script>

<template>
  <div class="parent">
    <div class="header">
      <h1>Datos mundiales</h1>
    </div>
    <div class="name">
      <label for="name">Inserta tu nombre:</label>
      <input type="text" v-model="name" id="name" />
      <p v-if="name.length > 0">
        Tu nombre <b>{{ name }}</b> tiene <b>{{ name.length }} letras</b>
      </p>
    </div>
    <div class="div-codes">
      <h2>Códigos de países</h2>
      <ul>
        <li v-for="code in codes" :key="code.code" @click="selectCountry(code)">
          {{ getCountryName(code) }}
        </li>
      </ul>
    </div>
    <div class="selected">
      <h2>Países seleccionados ({{ selectedCountries.length }})</h2>
      <p v-if="selectedCountries.length === 0">
        Debes seleccionar algún país en el panel de códigos.
      </p>
      <ul>
        <li v-for="code in sortedSelectedCountries" :key="code">
          <span>{{ getCountryName(code) }}</span>
          <button type="button" @click="unselectCountry(code)">Desmarcar</button>
        </li>
      </ul>
    </div>
    <div class="country-data">
      <CountryData :data="lastSelectedCountry" @remove-country="removeCountry" />
    </div>
    <div class="options">
      <h2>Opciones de gráfico</h2>
      <div>
        <label v-for="(label, mode) in chartModes" :key="mode">
          <input type="radio" :value="mode" v-model="chartMode" />
          {{ label }}
        </label>
      </div>
    </div>
    <div class="chart">
      <GoogleChart :data="chartCountries" />
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
  cursor: pointer;
}

ul li button {
  font-size: 0.6rem;
  margin: 2px;
}

h2 {
  font-size: 1.2rem;
}
</style>
