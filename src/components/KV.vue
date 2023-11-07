<script setup lang="ts">
import { ref,onMounted } from 'vue';

const emit = defineEmits<{(e:'setRootProperties',r:number,g:number,b:number):void}>()

const imgUrl=ref('')
const isCanvasElement = (element: HTMLElement | null): element is HTMLCanvasElement => {
  return element instanceof HTMLCanvasElement;
}
const uploadImage = (e: Event) => {
  const target = e.target as HTMLInputElement;
  if (target.files && target.files[0]) {
    const image = target.files[0];
    const reader = new FileReader();
    reader.readAsDataURL(image);
    reader.onload = (e) => {
      if (e.target && typeof e.target.result === 'string') {
        imgUrl.value = e.target.result;
      }else{
        imgUrl.value=""
      }
    };
  }
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
  <div class="KV">
    <img id="banner" :src="imgUrl" crossOrigin/>
    <div class="upload_btn" :class="{'fixed':imgUrl}">
      <input type="file" accept="image/jpeg" @change=uploadImage>
      <span>圖片格式:1920 x 600px</span>
    </div>
    <canvas id="source" width="1920" height="600"></canvas>
  </div>
</template>

<style scoped>

.upload_btn{
  position: fixed;
  top:300px;
  left:50%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  transition: all .3s;
  &.fixed{
    top:10px;
    left:10px;
  }
  & span{
    font-size: 12px;
    margin-top: 10px;
  }
}
.KV{
  width: 100%;
  height:var(--content-height);
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: antiquewhite;
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
