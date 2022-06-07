<script setup>
import ImageComponent from "./components/Image.vue";
import { ref, onMounted } from 'vue';

const images = ref([]);
var timer;
var canvas, ctx;
var imageElements = [];

onMounted(() => {
  canvas = document.querySelector('canvas');
  ctx = canvas.getContext("2d");
  redraw();
})

const w = 274;
const h = 400;

function redraw() {
  canvas.height = (Math.floor(imageElements.length / 4) + 1) * h;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  imageElements.forEach((imageElement, index) => {
    ctx.drawImage(imageElement, imageElement.width - w, 0, w, h, (index % 4) * w, Math.floor(index / 4) * h, w, h);
  });
}

// function resizeCanvas() {
//   // canvas.width = window.innerWidth;
//   // canvas.width = Math.floor(window.innerWidth / 4) * 4;
//   // canvas.height = window.innerHeight;
//   redraw();
// }

function typing(value) {
  clearTimeout(timer);
  timer = setTimeout(() => {
    images.value = value.split('\n');
    imageElements = [];
    images.value.forEach((_image, index) => {
      let im = new Image();
      im.src = _image;
      im.onload = function () {
        imageElements.push(im);
        if (imageElements.length == images.value.length) {
          redraw();
        }
      }
    });
    redraw();
  }, 200);
}
</script>

<template>
  <div class="m-4">
    <h1 class="text-3xl">kcompare</h1>
    <textarea @keyup="typing($event.target.value)" class="w-full min-h-[200px]"></textarea>
  </div>
  <canvas ref="canvas" width="1096" height="1000" class="w-full"></canvas>
  <div id="imagedisplay" class="flex justify-center flex-wrap mx-2">
    <ImageComponent v-for="image in images" :url="image" />
  </div>
</template>
