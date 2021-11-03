<template>
  <test />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import Test from '@/components/Test.vue';

export default defineComponent({
  name: 'App',
  components: {
    Test
  }
});
</script>

<style lang="scss">
html, body {
  margin: 0;
  padding: 0;
}
</style>
