<template>
    <div>
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, index) in answers" 
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="[selectedIndex === index ? 'selected' : '']"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button 
            :disabled="selectedIndex === null"
            variant="primary"
            @click="submitAnswer"
            >
                Submit Answer
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function, 
        increment: Function
    }, 
    data() {
        return {
            selectedIndex: null, 
            correctIndex: null, 
            shuffledAnswers: []
        }
    },
    computed: {
        answers() {
            return this.shuffledAnswers
        }
    },
    watch: {
        currentQuestion: {
            immediate: true, 
            handler(){
                this.shuffleAnswers()
                this.selectedIndex = null
            } 
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        }, 
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }, 
        submitAnswer() {
            let isCorrect = false 
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            } 
            this.increment(isCorrect)
            this.next()
        }
    }
}
</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }

    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }

    .btn {
        margin: 0 5px;
    }

    .selected {
        background-color: rgba(0, 195, 255, 0.493); 
    }

    .correct {
        background-color: rgba(0, 128, 6, 0.527); 
    }

    .incorrect {
        background-color: rgba(255, 0, 0, 0.192); 
    }
</style>