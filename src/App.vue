<script setup lang="ts">
import KV from './components/KV_Random.vue'

const setRootProperties = (r:number,g:number,b:number)=>{
  const root=document.documentElement
  root.style.setProperty('--background-primary',`rgb(${r},${g},${b})`);
}

</script>

<template>
  <KV @set-root-properties="setRootProperties"/>
</template>

<style scoped>
</style>
