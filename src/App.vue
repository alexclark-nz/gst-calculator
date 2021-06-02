<template>
  <main class="max-w-sm mx-auto p-6 flex flex-col min-h-screen justify-center items-center">
    <h1 class="mt-4 sm:mt-auto font-black text-4xl mb-6 text-green-400 text-center">New&nbsp;Zealand GST&nbsp;Calculator</h1>
    <Calculator />
    <p class="text-xs mt-auto">Created by <a target="_blank" href="https://alexclark.co.nz">Alex Clark</a></p> 
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Calculator from './components/Calculator.vue'

export default defineComponent({
  name: 'App',
  components: {
    Calculator
  }
})
</script>