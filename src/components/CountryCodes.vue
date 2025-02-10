<template>
  <div>
    <h2>Código de País</h2>
    <ul>
      <li v-for="(code) in countryCodes" :key="code" @click="$emit('select', code)">{{ code }}</li>
    </ul>
  </div>
</template>

<script>
import codes from '../../data/code.json';
export default {
  data() {
    return { countryCodes: codes };
  },
};
</script>


