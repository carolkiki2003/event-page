<script setup lang="ts">
import KV from './components/KV.vue'

const setRootProperties = (r:number,g:number,b:number)=>{
  const root=document.documentElement
  root.style.setProperty('--red',`${r}`);
  root.style.setProperty('--green',`${g}`);
  root.style.setProperty('--blue',`${b}`);
}

</script>

<template>
  <KV @set-root-properties="setRootProperties"/>
</template>

<style scoped>
</style>
