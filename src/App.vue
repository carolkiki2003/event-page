<script setup lang="ts">
import { ref,onMounted } from 'vue';
import KV from './components/KV.vue'

onMounted(()=>{
  draw()
})
const imgUrl=ref('https://picsum.photos/id/43/1920/600')
const previewImage = ref<string>('');

const isCanvasElement = (element: HTMLElement | null): element is HTMLCanvasElement => {
  return element instanceof HTMLCanvasElement;
}
const setRootProperties = (r:number,g:number,b:number)=>{
  const root=document.documentElement
  root.style.setProperty('--background-primary',`rgb(${r},${g},${b})`);
}
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
          setRootProperties(imgData[0],imgData[1],imgData[2])
      }
    }
  }
}
const changeImage = () => {
    let id =Math.floor(Math.random() * 501)
    imgUrl.value = `https://picsum.photos/id/${id}/1920/600`
      draw();
};
const uploadImage = (e: Event) => {
      const target = e.target as HTMLInputElement;
      if (target.files && target.files[0]) {
        const image = target.files[0];
        const reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = (e) => {
          if (e.target && typeof e.target.result === 'string') {
            previewImage.value = e.target.result;
            console.log(previewImage.value);
          }else{
            previewImage.value=""
          }
        };
      }
}
</script>

<template>
  <button class="upload_btn" @click="changeImage">Change Image</button>
  <KV :imgUrl="imgUrl"/>
  <div>
      <img class="uploading-image" :src="previewImage"/>
      <input type="file" accept="image/jpeg" @change=uploadImage>
   </div>
</template>

<style scoped>
.upload_btn{
  position: fixed;
  top:10px;
  left:10px;
}
</style>
