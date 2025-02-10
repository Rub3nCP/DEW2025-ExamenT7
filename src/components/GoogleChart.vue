<script setup>
import { GChart } from 'vue-google-charts';
import { ref, computed, reactive } from "vue";

const props = defineProps({
    "data": Array,
});

const chartTypes = [
    'ColumnChart',
    'BarChart',
    'GeoChart',
    'PieChart',
    'DonutChart',
];

const chartType = ref('DonutChart');

const realChartType = computed(() => {
    return chartType.value=='DonutChart' ? 'PieChart' : chartType.value;
});

const settings = { packages: ['geochart'] };

const options = reactive({
    width: 800,
    height: 600,
    pieHole: computed(() => chartType.value=='DonutChart' ? 0.4 : 0)
});

</script>

<template>
    <select v-model="chartType">
        <option v-for="type in chartTypes">{{ type }}</option>
    </select>
    <div>
        <GChart v-if="chartType=='GeoChart'" type="GeoChart" :data="data" :options="options" :settings="settings" />
        <GChart v-else :type="realChartType" :data="data" :options="options" />
    </div>
</template>

<style scoped>
div {
    margin: 0px auto;
    width: fit-content;
}
</style>