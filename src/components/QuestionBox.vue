<template>
    <div>
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item v-for="(answer, index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" :class="answerClass(index)">
            {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</b-button>
            <b-button @click="next" variant="success" :disabled="!answered">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false
            }
        },
        props: {
            currentQuestion: Object || {},
            next: Function,
            increment: Function
        },
        computed: {
            answers: function() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.answered = false;
                    this.shuffleAnswers()
                }
            }
        },
        methods: {
            answerClass: function(index) {
                let answerClass = '';

                if(!this.answered  && this.selectedIndex === index) {
                    answerClass = 'answer-selected';
                } else if(this.answered && this.correctIndex === index) {
                    answerClass = 'answer-correct';
                } else if(this.answered && this.correctIndex !== index && this.selectedIndex === index) {
                    answerClass = 'answer-incorrect';
                }

                return answerClass;
            },
            selectAnswer: function(index) {
                if(!this.answered) {
                    this.selectedIndex = index;
                }
            },
            submitAnswer: function() {
                let isCorrect = false
                if(this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true;

                this.increment(isCorrect)
            },
            shuffleAnswers: function () {
                let array = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                const length = array == null ? 0 : array.length
                if (!length) {
                    return []
                }
                let index = -1
                const lastIndex = length - 1
                const result = array
                while (++index < length) {
                    const rand = index + Math.floor(Math.random() * (lastIndex - index + 1))
                    const value = result[rand]
                    result[rand] = result[index]
                    result[index] = value
                }
                this.shuffledAnswers = result
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            }
        }
    }
</script>

<style scoped>
    .list-group {
        margin-bottom: 20px;
    }
    .list-group-item:hover {
        background-color: #eeeeee;
        cursor: pointer;
    }
    .btn {
        margin: 0 5px;
    }
    .answer-selected {
        background-color: #428bca;
    }
    .answer-correct {
        background-color: #5cb85c;
        color: #ffffff;
    }
    .answer-incorrect {
        background-color: #d9534f;
        color: #ffffff;
    }
</style>