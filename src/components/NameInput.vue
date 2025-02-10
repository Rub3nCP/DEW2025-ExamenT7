<template>
  <div>
    <input v-model="name" type="text" placeholder="Escribe tu nombre">
    <p v-if="name">El nombre es {{ name }} y tiene {{ name.length }} letras.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: ""
    };
  }
};
</script>
