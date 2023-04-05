<script setup>
import { ref, onMounted, watch } from "vue";

const images = ref([]);
var timer;
var canvas, ctx;
var imageElements = [];

onMounted(() => {
  canvas = document.querySelector("canvas");
  ctx = canvas.getContext("2d");
  redraw();
});

const w = 274;
const h = 400;
const cardsPerRow = ref(4);

watch(cardsPerRow, () => {
  redraw();
});

function redraw() {
  canvas.width = cardsPerRow.value * w;
  canvas.height =
    (Math.floor((imageElements.length - 1) / cardsPerRow.value) + 1) * h;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  imageElements.forEach((imageElement, index) => {
    if (imageElement.width >= w || imageElement.height >= h) {
      ctx.drawImage(
        imageElement,
        imageElement.width - w,
        0,
        w,
        h,
        (index % cardsPerRow.value) * w,
        Math.floor(index / cardsPerRow.value) * h,
        w,
        h
      );
    }
  });
}

var loaded = 0;

function typing(value) {
  textarea.value.style.height = "auto";
  textarea.value.style.height = textarea.value.scrollHeight + "px";
  clearTimeout(timer);
  timer = setTimeout(() => {
    imageElements = [];
    if (value.length == 0) {
      images.value = [];
      redraw();
    } else {
      images.value = value.split("\n");
    }
    loaded = 0;
    images.value.forEach((_image) => {
      let im = new Image();
      imageElements.push(im);
      im.src = _image;
      im.onload = function () {
        loaded++;
        if (loaded >= images.value.length) {
          redraw();
        }
      };
      im.onerror = function () {
        loaded++;
        if (loaded >= images.value.length) {
          redraw();
        }
      };
    });
  }, 200);
}

const textarea = ref(null);

function fillExampleInput() {
  if (
    textarea.value.value.length == 0 ||
    confirm("Are you sure you want to replace the input?")
  ) {
    textarea.value.value = `https://cdn.discordapp.com/attachments/887049782579855410/982890744744378378/h9f6x4-gildedstars.png
https://cdn.discordapp.com/attachments/887049782579855410/982890745029599302/h9f6x4-rainforest.png
https://cdn.discordapp.com/attachments/887049782579855410/982890745377730560/h9f6x4-springlilies.png
https://cdn.discordapp.com/attachments/887049782579855410/982890745730068520/h9f6x4-shadowglass.png
https://cdn.discordapp.com/attachments/887049782579855410/982890746044633148/h9f6x4-tiara.png
https://cdn.discordapp.com/attachments/887049782579855410/982890746317254677/h9f6x4-budokai.png`;
    typing(textarea.value.value);
  }
}
</script>

<template>
  <div class="bg-[#36393f] min-h-screen flex flex-col">
    <div class="p-4">
      <h1 class="text-3xl text-gray-100 mb-1">kcompare</h1>
      <h2 class="text-gray-300 mb-3">
        Compare <a href="https://karuta.com/" class="link">Karuta</a>'s
        <span class="command">kv</span>, <span class="command">ku</span>,
        <span class="command">kdye</span>, <span class="command">km</span>,
        <span class="command">kmr</span>, <span class="command">kfr</span>,
        <span class="command">kdr</span>, <span class="command">ktrr</span>,
        <span class="command">kalr</span>, and
        <span class="command">ksr</span> image outputs. Just paste image links
        separated by new lines into this textarea below! Click
        <span class="link" @click="fillExampleInput()">here</span> for an
        example input.
      </h2>
      <textarea
        ref="textarea"
        @keyup="typing($event.target.value)"
        class="w-full min-h-[200px] bg-gray-500 outline-none text-gray-200 px-3 py-1 whitespace-nowrap placeholder:whitespace-normal overflow-y-hidden resize-none"
        placeholder="Lines of image links here..."
      ></textarea>
      <div>
        <label for="cards" class="text-gray-300 mr-2">Cards per row</label>
        <input
          type="number"
          id="cards"
          name="cards"
          v-model="cardsPerRow"
          class="bg-gray-500 outline-none text-gray-200 px-3 py-1"
          min="1"
        />
      </div>
    </div>
    <canvas
      ref="canvas"
      width="1096"
      height="300"
      class="w-full p-2 max-w-3xl mx-auto"
      >YOUR BROWSER DOESNT SUPPORT CANVASES?!?!?!</canvas
    >
    <footer class="p-2 text-gray-300 mt-auto">
      Created by
      <a href="https://realcyguy.netlify.app" class="link">Cyrus Yip</a>, open
      source on
      <a href="https://github.com/realcyguy/kcompare" class="link">GitHub</a>,
      join my
      <a href="https://notbad.netlify.app/support" class="link">Discord</a>!
    </footer>
  </div>
</template>
