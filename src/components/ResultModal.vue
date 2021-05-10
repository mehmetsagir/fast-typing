<template>
  <div class="h-screen w-screen fixed top-0 left-0 flex items-center justify-center bg-black bg-opacity-80" v-show="viewModal">
    <div class="bg-gray-200 md:w-8/12 lg:w-4/12 w-11/12 rounded-md mt-4 py-3">
      <div class="flex items-end mr-5">
         <button type="button" class="ml-auto bg-white rounded-md p-2 inline-flex items-center justify-center text-gray-400 hover:text-gray-500 hover:bg-gray-100" @click="viewModal = false">
              <span class="sr-only">Close menu</span>
              <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
        </button>
      </div>
      <h1 class="text-5xl font-bold text-center text-green-700 py-6">{{ dks }} DKS</h1>
      <ul>
        <li class="px-9 py-3 flex justify-between border-b border-gray-300">
            <span>Doğruluk</span>
            <span class="pr-5 font-medium">{{ truthRate }}%</span>
        </li>
        <li class="px-9 py-3  flex justify-between border-b border-gray-300">
            <span>Doğru kelime</span>
            <span class="pr-5 text-green-700 font-medium"> {{ trueCount }} </span>
        </li>
        <li class="px-9 py-3  flex justify-between">
            <span>Yanlış kelime</span>
            <span class="pr-5 text-red-700 font-medium"> {{ falseCount }} </span>
        </li>
      </ul>
      <div class="text-right mt-6 px-6">
        <button class="btn-primary">
          Kaydet
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResultModal",
  data () {
    return {
      falseCount: 0,
      trueCount: 0,
      viewModal: false
    }
  },
  props: {
    falseCounter: {
      require: true,
      type: Number
    },
    trueCounter: {
      require: true,
      type: Number
    },
    isRunning: {
      require: true,
      type: Boolean
    }
  },
  watch: {
    falseCounter() {
      if(this.isRunning) this.falseCount = this.falseCounter
    },
    trueCounter() {
      if(this.isRunning) this.trueCount = this.trueCounter
    },
    isRunning() {
      if(!this.isRunning) this.viewModal = true  
    },
    viewModal() {
      this.$emit('nonWriting', this.viewModal)
    }
  },  
  computed: {
    dks() {
      return this.falseCount + this.trueCount
    },
    truthRate() {
      return Math.floor(((100 / this.dks) * this.trueCount)) || 0
    }
  },
  created() {
    if (this.viewModal) {
      document.addEventListener('keyup', e => {
        if(e.keyCode === 27) this.viewModal = false
      })
    }
  }
};
</script>