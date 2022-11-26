<script setup>
import { reactive, ref } from 'vue';
import DateWindow from '../components/DateWindow.vue';
import {
    Dialog,
    DialogPanel,
    DialogTitle,
    DialogDescription,
  } from '@headlessui/vue'
  const isOpen = ref(true)

  function setIsOpen(value) {
    console.log('here')
    isOpen.value = value
  }
const dates = reactive([
  {
    date: new Date(2022, 12, 1),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 2),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 3),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 4),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 5),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 6),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 7),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 8),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 9),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 10),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 11),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 12),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 13),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 14),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 15),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 16),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 17),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 18),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 19),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 20),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 21),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 22),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 23),
    message: 'Merry Xmas!',
    opened: false
  },
  {
    date: new Date(2022, 11, 24),
    message: 'Merry Xmas 2!',
    opened: true
  },
  {
    date: new Date(2022, 12, 25),
    message: 'Merry Xmas!',
    opened: false
  },
])
</script>

<template>
  <main class="mx-auto p-8  h-screen bg-cover" style="background-image: url('/christmas3.jpg')">

    <div class="rounded-lg p-8 ">
      <h1 @clik="setIsOpen()" class="text-4xl font-bold text-blue-200  mt-4 mb-20 text-center">
        Amber's Advent Calendar
      </h1>

      <div class="grid grid-cols-5 gap-12 justify-items-center">
        <div v-for="date in dates" class="w-12 sm:w-20 md:w-28 lg:w-36 xl:w-44 2xl:w-56 h-12 sm:h-20 md:h-28 lg:h-36 xl:h-44 2xl:h-56 shadow-inner-2xl rounded-lg bg-blue-400">
          <DateWindow :date="date" />
        </div>
      </div>

    </div>
  </main>
  
</template>

<style scoped>

</style>
