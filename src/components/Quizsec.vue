<template>
   <div class="card w-50 m-auto">
        <div class="card-body bg-light">
            <h5 class="card-title">
                {{ currentQuestion.question }}
            </h5>
            <hr>
            <ul class="list-group">
                <li class="list-group-item mb-3" 
                    v-for="(answer, index) in answers" :key="index"
                    @click.prevent="selectedAnswer(index)"
                    :class="[!answered && selectedIndex === index ? 
                    'selected' : answered && correctIndex === index ? 
                    'correct' : answered && selectedIndex === index && correctIndex !== index ? 'incorrect' : '']"
                >
                    {{ answer }}
                </li>
            </ul>
            <button class="btn btn-primary mr-2" @click="submitAnswer" :disabled = "selectedIndex === null || answered">
                SUBMIT
            </button>
            <button class="btn btn-success" @click="next">
                NEXT
            </button>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
    },

    data () {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered: false
        }
    },

    // watches for changes in props
    // We can call the mounted life cycle to shuffle the answers on first reload also but this is a better way.
    watch: {
      currentQuestion: {
          immediate: true,
          handler: function() {
              this.shuffleAnswer()
              this.selectedIndex = null
              this.answered = false 
          }
      }
    },


    methods: {
        selectedAnswer(index) {
            this.selectedIndex = index
        },

        shuffleAnswer() {
            const answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        
        submitAnswer() {
            let isCorrect = false 

            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
    },

    computed: {
        answers() {
            // used  '...' to seperate each array into a different array
            const answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    }
}
</script>

<style scoped>
    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }

    .selected {
        background: lightblue;
    }

    .correct {
        background: lightgreen ;
    }

    .incorrect {
        background:lightsalmon;
    }
</style>