<template>
  <div>
    <GoogleChart :chartData="chartData" />
  </div>
</template>

<script>
import GoogleChart from '../components/GoogleChart.vue';

export default {
  components: {
    GoogleChart,
  },
  props: {
    chartData: {
      type: Array,
      required: true,
    },
  },
};
</script>
