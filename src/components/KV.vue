<script setup lang="ts">

defineProps<{ imgUrl:string}>()

</script>

<template>
  <div class="KV">
    <img id="banner" :src="imgUrl" crossOrigin>
    <canvas id="source" width="1920" height="600"></canvas>
  </div>
</template>

<style scoped>
.KV{
  width: 100%;
  aspect-ratio: 16/5;
}
#banner{
  width: 100%;
}
canvas{
  display:none;
}
</style>
