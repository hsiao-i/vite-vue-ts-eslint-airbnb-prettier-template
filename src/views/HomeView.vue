<script setup lang="ts">
import { ref } from 'vue';
import TestCom from '@/components/TestCom.vue';
import type { Test } from '@/types/typeCategories';

const msg = ref<Test>({
  id: 15,
  name: 'Vite',
  age: 18,

});
console.log(msg.value);
</script>

<template>
  <main>
    Vite
    <TestCom />
  </main>
</template>
