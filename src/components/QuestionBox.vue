<template>
  <!-- <div class="question-box-container"> -->
  <b-jumbotron>
      
    <template #lead>
      <br>
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">
    <div class="container">
      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in shuffledAnswers" 
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"     
          > {{ answer }}
        </b-list-group-item>
      </b-list-group>
    </div>
    <br>
    <b-button variant="primary" 
      @click="submitAnswer"
      :disabled="answered || selectedIndex == null"
    
    >Submit</b-button>
    <b-button variant="success" @click="changeIndex"
              :disabled="!answered"
    >Next</b-button>
  </b-jumbotron>
<!-- </div> -->
</template>

<script>
import _ from 'lodash';

export default {
  
    props: { 
        currentQuestion: Object,
        changeIndex: Function,
        index: Number,
        increment: Function
    },
    data() {
      return {
        selectedIndex: null,
        shuffledAnswers: [],
        correctIndex: null,
        answered: false,
        interval: null,
        time: 0
        
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers];   
        answers.push(this.currentQuestion.correct_answer)
        return answers
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler(){
          this.selectedIndex = null
          this.shuffleAnswers()
          this.answered = false
           
        }
        
      }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
        console.log(this.selectedIndex)
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)

        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
          console.log(this.correctIndex)
          console.log(answers, this.shuffledAnswers)
      },
      submitAnswer(){
          let isCorrect = false
          console.log(this.correctIndex)
          if(this.selectedIndex == this.correctIndex) {
            isCorrect = true
          }
          this.increment(isCorrect)
          this.answered = true
          this.disableClick = true
      },
      answerClass(index) {
        let answerClass = {}
        if (!this.answered && this.selectedIndex === index) {
          answerClass = {'selected': 'selected'}
        } else if(this.answered && this.correctIndex === index) {
           answerClass = {
             'correct': 'correct',
             'froze': 'froze'
           }
        } else if (this.answered && this.selectedIndex===index && this.correctIndex !== index){
          console.log('incorrect class works')
          answerClass = {
              'incorrect': 'incorrect',
              'froze': 'froze'  }
        } else if (this.answered && this.selectedIndex !== index && this.correctIndex !== index){
          answerClass = { 'froze': 'froze'}
        }

        return answerClass
          
          
      }
      
  }
}
</script>

<style scoped>
.container {
  max-width: 300px;
}
.list-group{
  margin-bottom: 24px;
}

.list-group-item:hover {
  background-color: #EEE;
  cursor: pointer;
}
.selected {
  background-color: lightblue;
  
}
.froze {
  pointer-events: none
}
.correct {
  background-color: lightgreen;
  pointer-events: none
}
.incorrect {
  background-color: red;
  pointer-events: none
}

</style>