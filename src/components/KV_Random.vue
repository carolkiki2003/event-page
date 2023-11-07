<script setup lang="ts">
import { ref,onMounted } from 'vue';

const emit = defineEmits<{(e:'setRootProperties',r:number,g:number,b:number):void}>()

const imgUrl=ref('https://picsum.photos/id/43/1920/600')
const isCanvasElement = (element: HTMLElement | null): element is HTMLCanvasElement => {
  return element instanceof HTMLCanvasElement;
}
const changeImage = () => {
    let id =Math.floor(Math.random() * 501)
    imgUrl.value = `https://picsum.photos/id/${id}/1920/600`
      draw();
};
const draw = ()=>{
  const element = document.getElementById('source')
  if (isCanvasElement(element)) {
    const canvasElement = element;
    const ctx = canvasElement.getContext('2d');
    const img = document.getElementById('banner')
    if (canvasElement && ctx && img instanceof HTMLImageElement ){
      img.onload = () => {
          ctx.drawImage(img,0, 0)
          let imgData = (ctx.getImageData(0, 590, 1920, 10)).data
          emit('setRootProperties', imgData[0],imgData[1],imgData[2])
      }
    }
  }
}
onMounted(()=>{
  draw()
})

</script>

<template>
  <button class="change_btn" @click="changeImage">Change Image</button>
  <div class="KV">
    <img id="banner" :src="imgUrl" crossOrigin>
    <canvas id="source" width="1920" height="600"></canvas>
  </div>
</template>

<style scoped>

.change_btn{
  position: fixed;
  top:10px;
  left:10px;
}
.KV{
  width: 100%;
  height:var(--content-height);
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}
#banner{
  object-fit: cover;
}
canvas{
  display:none;
}
@media screen and (min-width:1920px) {
  #banner{
    width: 100%;
  }
}
/* test */
@media screen and (max-width:767px){
  #banner{
    object-fit: cover;
    object-position: -200px 50%;
  }
}
</style>
