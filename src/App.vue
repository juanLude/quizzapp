<template>
  <div id="app">
    
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <QuestionBox 
          v-if="questions.length && index<=9"
          :currentQuestion="questions[index]" 
          :changeIndex="changeIndex"
          :index="index"
          :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
    <Result
    v-if="index>9"
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    :time="time"
    :interval="interval"
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Result from './components/Result.vue'

export default {
  name: 'App',
  components: {
    Header, QuestionBox, Result
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      time: 0,
      interval: null
    }
  },
  methods: {
      changeIndex() {
         this.index ++
        console.log(this.index)
      },
      increment(isCorrect) {
        if(isCorrect) {
          this.numCorrect++
          
        }
        this.numTotal++
      },
      countTime() {
        this.interval = setInterval(this.start, 1000);
        console.log('time has started')
      },
      start() {
          this.time = parseInt(this.time) + 1;
          console.log(this.time)
        }
      },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    }).then(response => {
      return response.json();
    }).then(data => {
      this.questions = data.results
      // console.log(data.results[this.index].incorrect_answers)
      // console.log(data.results[this.index].correct_answer)
    })
    this.countTime()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
