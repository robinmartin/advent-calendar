<template>
  <div ref="parent"
    class="relative flex self-center justify-center items-center rounded-lg shadow-inner bg-gray-100/40 shadow-gray-900">
    <i v-if="isFuture" class="z-40 absolute top-0 left-0 text-white fa fa-lock text-xs sm:text-md md:text-lg lg:text-xl sm:px-2 sm:py-1"/>
    <canvas class="z-20 rounded-lg shadow-2xl" ref="myCanvas" @click="showDatePopup" @mousedown="canvasDraw"
      @mousemove="mouseMove" @mouseup="finishDraw" @touchmove="touchMove" :width="width" :height="height">
    </canvas>
    <div class="absolute text-center rounded-lg  flex z-10 justify-center items-center">
      <i class="text-green-700 text-3xl sm:text-4xl md:text-5xl lg:text-6xl 2xl:text-7xl fa-solid " :class="date.icon"></i>
    </div>
  </div>
</template>

<script setup>
import { useMouse } from "@vueuse/core";
import moment from "moment";
import { computed, defineEmits, defineProps, onMounted, onUpdated, reactive, ref } from "vue";

const emit = defineEmits(['opened'])

const props = defineProps({
  date: {
    type: Object,
    required: true,
  },
});

const today = moment();

const isPast = computed(() => {
  return props.date.date.isBefore(today, "day");
})

const isToday = computed(() => {
  return props.date.date.isSame(today, "day");
})

const isFuture = computed(() => {
  return props.date.date.isAfter(today, "day");
})


const getDay = () => {
  return props.date.date.format("D");
};


const finished = ref(false);
const showDatePopup = () => {
  if (finished.value || isPast.value) {
    emit('opened', props.date)
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
  ctx.canvas.style.opacity = isPast.value ? 0 : 1;
  ctx.textAlign = "center";
  ctx.textBaseline = 'middle';
  ctx.font = '48px Mountains of Christmas';
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
  console.log(isToday.value)
  if (isToday.value) {
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
  }

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
      // showDatePopup()
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


