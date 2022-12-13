<script setup>
import {
  Dialog,
  DialogPanel,
  DialogTitle
} from '@headlessui/vue';
import moment from 'moment';
import { reactive, ref } from 'vue';
import DateWindow from '../components/DateWindow.vue';


const open = ref(false)

function daysToChristmas() {
  return moment('2022/12/25').diff(moment(), 'days');
}

const modalData = reactive({
  date: null,
})

function showModal(date) {
  console.log(date)
  modalData.date = date;
  open.value = true
}
const dates = reactive([
  {
    date: moment("2022/12/1"),
    message: 'Look in the coldest place in the house',
    icon: 'fa-sleigh'
  },
  {
    date: moment("2022/12/2"),
    message: 'Where do all the bananas, apples and oranges live?s',
    icon: 'fa-candy-cane'
  },
  {
    date: moment("2022/12/3"),
    message: 'What do chickens lay?',
    icon: 'fa-gift'
  },
  {
    date: moment("2022/12/4"), // Sunday
    message: 'Where does Santa sometimes get stuck?',
    icon: 'fa-holly-berry'
  },
  {
    date: moment("2022/12/5"), // Monday
    message: 'Who wants to go for a bike ride?',
    icon: 'fa-cross'
  },
  {
    date: moment("2022/12/6"), // Tuesday
    message: 'What\s Wrennie\'s favourite book?',
    icon: 'fa-tree'
  },
  {
    date: moment("2022/12/7"), // Wednesday
    message: 'Tickets to the wee show',
    icon: 'fa-sleigh'
  },
  {
    date: moment("2022/12/8"), // Thursday
    message: 'Where the post comes in',
    icon: 'fa-candy-cane'
  },
  {
    date: moment("2022/12/9"), // Friday
    message: 'Has Heart got a gift for you?',
    icon: 'fa-gift'
  },
  {
    date: moment("2022/12/10"), // Saturday
    message: 'Go to Amber\'s office!',
    icon: 'fa-holly-berry'
  },
  {
    date: moment("2022/12/11"), // Sunday
    message: 'What is cooking in Wrennie\'s kitchen?',
    icon: 'fa-cross'
  },
  {
    date: moment("2022/12/12"), // Monday
    message: 'On top of your wardrobe!',
    icon: 'fa-tree'
    
  },
  {
    date: moment("2022/12/13"), // Tuesday 
    message: 'Under the tree',
    icon: 'fa-sleigh'
  },
  {
    date: moment("2022/12/14"), // Wednesday
    message: 'Mummy\'s computer',
    icon: 'fa-fa-candy-cane'
  },
  {
    date: moment("2022/12/15"), // Thursday
    message: 'In the window',
    icon: 'fa-gift'
  },
  {
    date: moment("2022/12/16"), // Friday
    message: 'On Daddy\'s desk',
    icon: 'fa-holly-berry'
  },
  {
    date: moment("2022/12/17"), // Saturday
    message: '',
    icon: 'fa-cross'
  },
  {
    date: moment("2022/12/18"), // Sunday
    message: '',
    icon: 'fa-tree'
  },
  {
    date: moment("2022/12/19"), // Monday
    message: '',
    icon: 'fa-sleigh'
  },
  {
    date: moment("2022/12/20"), // Tuesday
    message: '',
    icon: 'fa-candy-cane'
  },
  {
    date: moment("2022/12/21"), // Wednesday
    message: '',
    icon: 'fa-holly-berry'
  },
  {
    date: moment("2022/12/22"), // Thursday
    message: '',
    icon: 'fa-cross'
  },
  {
    date: moment("2022/12/23"), // Friday
    message: '',
    icon: 'fa-gift'
  },
  {
    date: moment("2022/12/24"), // Saturday
    message: '',
    icon: 'fa-sleigh'
  },
  {
    date: moment("2022/12/25"),
    message: '',
    icon: 'fa-gift'
  },
])
</script>

<template>
  <main class="mx-auto p-8  min-h-screen bg-cover" style="background-image: url('/christmas3.jpg')">




    <div class="rounded-lg p-8 ">
      <h1 class="text-3xl sm:text-4xl md:text-6xl lg:text-7xl font-bold text-blue-200  mt-4 mb-20 text-center">
        Amber's Advent Calendar
      </h1>

      <div class="grid grid-cols-5 gap-12 justify-items-center">
        <div v-for="date in dates"
          class="w-12 sm:w-20 md:w-28 lg:w-36 xl:w-44 2xl:w-56 h-12 sm:h-20 md:h-28 lg:h-36 xl:h-44 2xl:h-56  rounded-lg">
          <DateWindow :date="date" @opened="showModal(date)" />
        </div>
      </div>

      <div class="mt-16 text-center text-white text-4xl sm:text-5xl md:text-6xl lg:text-7xl font-bold ">
        {{ daysToChristmas() }} more sleeps until christmas!
      </div>

    </div>
  </main>

  <TransitionRoot as="template" :show="open">
    <Dialog as="div" class="relative z-50" :open="open" @close="open = false">
      <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100"
        leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
      </TransitionChild>

      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
          <TransitionChild as="template" enter="ease-out duration-300"
            enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200"
            leave-from="opacity-100 translate-y-0 sm:scale-100"
            leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
            <DialogPanel
              class="bg-blue-700/50 border-8 border-white w-fit relative transform overflow-hidden rounded-lg  px-4 pt-5 pb-4 text-left transition-all sm:my-8 sm:p-6">
              <div class="flex items-center">

                <div class="mt-4">
                    <img src="/elf.png" class="mx-auto h-96">
                  </div>
                <div class="text-center -ml-24 -mt-24">

                  <DialogTitle as="h3" class="text-3xl font-medium leading-6 text-gray-900 min-w-full bg-white rounded-lg py-24 px-8 shadow-xl">
                    <div class="text-center">
                      "{{ modalData.date.message }}"
                    </div>
                  </DialogTitle>
           
                  
                </div>
              </div>

            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>

</template>

<style scoped>

</style>
