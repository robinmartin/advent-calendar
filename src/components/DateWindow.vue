<template>
  <div ref="parent" class="relative flex self-center justify-center items-center rounded-lg">
    <canvas class="z-20 rounded-lg" ref="myCanvas" @click="toggleModal" @mousedown="canvasDraw" @mousemove="mouseMove" @mouseup="finishDraw"
      @touchmove="touchMove" :width="width" :height="height">
    </canvas>
    <div class="absolute text-center flex z-10 justify-center items-center">
      <button>
        <i class="text-blue-900 text-3xl sm:text-4xl md:text-5xl lg:text-6xl 2xl:text-7xl fa-solid fa-gift"></i>
      </button>

    </div>
  </div>
</template>

<script setup>
import { useMouse } from "@vueuse/core";
import moment from "moment";
import { defineProps, onMounted, onUpdated, reactive, ref } from "vue";

 

const props = defineProps({
  date: {
    type: Object,
    required: true,
  },
});

const getDay = () => {
  return moment(props.date.date).format("D");
};

const showModal = ref(false);
const finished = ref(false);
const toggleModal = () => {
  if (finished.value) {
    console.log('showing modal');
    showModal.value = !showModal.value;
  }
  
};
const myCanvas = ref(null);
const parent = ref(null);
const width = ref(0);
const height = ref(0);
const position = ref(1);
const scratchText = ref(getDay());
const imgSrc = ref(
  "https://www.niusnews.com/upload/imgs/default/202108_Jennie/0831catmeme/44.jpg"
);
const mouse = reactive(useMouse());

onMounted(() => {
  width.value = parent.value.clientWidth;
  height.value = parent.value.clientWidth;
  initCanvas();
});

onUpdated(() => {
  initCanvas();
});


const initCanvas = () => {
  const canvas = myCanvas.value;
  const ctx = canvas.getContext("2d");

  // draw background
  ctx.clearRect(0, 0, canvas.width, canvas.width);
  ctx.fillStyle = "#003A8A";
  ctx.fillRect(0, 0, width.value, width.value);

  // draw the text ie the date number
  ctx.fillStyle = "#fff";
  ctx.canvas.style.opacity = 1;
  ctx.textAlign = "center";
  ctx.textBaseline = 'middle';
  ctx.font = '36px sans-serif';
  ctx.fillText(scratchText.value, width.value / 2, height.value / 2);

  autoScratching();
};

const autoScratching = () => {
  const canvas = myCanvas.value;
  const ctx = canvas.getContext("2d");
  setTimeout(() => {
    console.log("autoScratching");
  }, 1000);
};

const scratching = () => {
  const { x, y } = getClientOffset(mouse);
  const canvas = myCanvas.value;
  const ctx = canvas.getContext("2d");
  const offsetX = x - 0.001;
  const offsetY = y - 0.001;

  ctx.globalCompositeOperation = "destination-out";
  ctx.lineJoin = "round";
  ctx.lineCap = "round";
  ctx.lineWidth = "30";
  ctx.beginPath();
  ctx.moveTo(x, y);
  ctx.lineTo(offsetX, offsetY);
  ctx.stroke();
  getFilledPercentage(ctx);
};


const getClientOffset = (position) => {
  if (myCanvas.value) {
    const rect = myCanvas.value.getBoundingClientRect();
    const point = {
      x: position.x - rect.left,
      y: position.y - rect.top,
    };
    return point;
  }
};

const getFilledPercentage = (ctx) => {
  let imgData;
  if (position.value === 1) {
    imgData = ctx.getImageData(0, 0, width.value, height.value);
  }
  if (position.value === 2) {
    imgData = ctx.getImageData(150, 0, width.value, height.value);
  }

  let pixels = imgData.data;
  let n = 0;
  for (let i = 0; i < pixels.length; i += 100) {
    if (pixels[i + 3] < 128) {
      n += 100;
    }
  }

  if (n >= pixels.length * 0.3) {
    ctx.globalCompositeOperation = "destination-over";
    fadeOut(ctx);
  }
};

const fadeOut = (ctx) => {
  let timesRun = 0;
  let interval = setInterval(() => {
    timesRun += 1;
    ctx.canvas.style.opacity *= 0.99;
    if (timesRun === 200) {
      clearInterval(interval);
      finished.value = true
      toggleModal()
    }
  }, 3);

};

let isDraw = false;
const canvasDraw = () => {
  isDraw = true;
};

const mouseMove = (e) => {
  if (!isDraw) return;
  scratching();
};

const finishDraw = () => {
  isDraw = false;
};

const touchMove = (e) => {
  const canvas = myCanvas.value;
  if (e.target === canvas) {
    e.preventDefault();
    scratching();
  }
};

</script>


