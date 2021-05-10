<template>
  <div>
    <div class="bg-gray-200 p-5 rounded mt-16">
      <div class="text-2xl py-4 flex flex-wrap max-h-44 overflow-hidden">
        <span v-for="(word, index) in words" :key="index"
          :class="index == 0 ? writingWordControl : null" class="px-2 py-1 opacity-60">{{ word }}</span>
      </div>
      <div class="py-4 flex">
        <input type="text" :readonly="nonWriting" class="h-10 px-2 flex-1 text-xl border-red-600" v-model="writingWord">
        <button class="btn-primary opacity-75 ml-2">{{timer}}sn.</button>
        <button class="btn-primary mx-2" @click="getWords">Yenile</button>
      </div>
    </div>
    <ResultModal @nonWriting="nonWriting = $event" :trueCounter="trueCount" :falseCounter="falseCount" :isRunning="isRunning" />
  </div>
</template>

<script>
import wordList from "../assets/words.json"
import ResultModal from './ResultModal.vue'
export default {
  name: "FastTyping",
  data() {
    return {
      writingWord: "",
      words: [],
      wordList: wordList,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 60,
      timerInterval: false,
      isRunning: false,
      isFinish: false,
      nonWriting: false
    }
  },
  components: { ResultModal },
  watch: {
    writingWord(val) {
      if (!val || val === ' ') { this.writingWord = ''; return }
      if(!this.isRunning) this.toggleTimer()

      const word = this.words[0].slice(0, val.length)
      const userWord = val.replace(' ' , '')

      this.isTrue = word === userWord

      if (val.indexOf(' ') !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++
        this.words.splice(0,1)
        this.writingWord = ""
        this.isTrue = true
      }
    }
  },
  mounted() {
    this.getWords()
  },
  computed: {
    writingWordControl() {
      return this.isTrue ? 'highlight' : 'danger'
    }
  },
  methods: {
    toggleTimer() {
      this.isRunning = true;
      this.timerInterval = setInterval(() => {
        if (this.timer === 0) {
          this.getWords()
          this.isFinish = true
          return
        }
        this.timer-- 
      }, 1000)
    },
    getWords () {
      this.isTrue = true
      this.isFinish = false
      this.isRunning = false
      this.timer = 60
      this.writingWord = ''
      this.trueCount = 0
      this.falseCount = 0 
      clearInterval(this.timerInterval)
      this.words = this.wordList.sort(() => Math.random() - 0.5).splice(0, 400).filter(item => item.length > 2 && item.length < 8).map(item => item.toLowerCase())
    }
  }
};
</script>

<style scoped>
.highlight {
  @apply 
  bg-gray-600 
  opacity-100 
  rounded 
  text-white
}
.danger {
  @apply 
  opacity-100 
  bg-red-600
  rounded 
  text-white
}
</style>
