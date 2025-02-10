<template>
  <div>
    <h2>Opciones gráficas</h2>
    <ul>
      <li v-for="option in options" :key="option" @click="$emit('select', option)">{{ option }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return { options: ['population', 'pib', 'area', 'income'] };
  },
};
</script>