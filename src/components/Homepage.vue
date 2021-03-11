<template>
    <div>
        <div class="button-Div">
            <b-button id="show-btn" @click="$bvModal.show('bv-modal-example')" :disabled="numTotal <= 0">Check Current Scores</b-button>

            <b-modal id="bv-modal-example" hide-footer>
                <template #modal-title>
                Current Scores
                </template>
                <div class="d-block text-center">
                <p>
                    Questions Answered Correctly: <span> {{ numCorrect }} </span>
                </p>
                <p>
                    Total Questions: <span> {{ numTotal }} </span>
                </p>
                <p class="percentageScore"> {{ percentageScore }}% </p>
                </div>
                <b-button class="mt-3" block @click="$bvModal.hide('bv-modal-example')">Close Me</b-button>
            </b-modal>
        </div>
        <div v-if="answered" class="mt-1">
            <div class="divCorrect text-center" v-if="correctIndex === selectedIndex">
                <b-alert show dismissible>
                    <p>Correct</p>
                    <b-icon icon="check-square" scale="2" variant="success"></b-icon>
                </b-alert>
            </div>
            <div class="divIncorrect text-center" v-else-if="correctIndex !== selectedIndex">
                <b-alert show dismissible>
                    <p>Incorrect</p>
                    <b-icon icon="x-circle" scale="2" variant="danger"></b-icon>
                </b-alert>
            </div>
        </div>
        <b-card
            :title="currentQuiz.category"
            tag="article"
            style="max-width: 30rem; margin: auto; margin-top: 10px;"
            class="mb-2"
        >
            <b-form-group
                id="fieldset-horizontal"
                label-cols-sm="4"
                label-cols-lg="3"
                content-cols-sm
                content-cols-lg="7"
                label="Category"
                label-for="selectedCategory"
                >
            <b-form-select v-model="selectedCategory" :options="options"></b-form-select>
            </b-form-group>
            <b-card-text v-html="currentQuiz.question"></b-card-text>
            <b-list-group class="mb-3">
                <b-list-group-item 
                    v-for="(answer, index) in answers" 
                    :key="index" 
                    v-html="answer" 
                    @click.prevent="getSelectedIndex(index)" 
                    :class="answeredClass(index)"
                >
                </b-list-group-item>
            </b-list-group>
            <b-button href="#" variant="primary" @click="submitAnswer" :disabled="selectedIndex == null || answered">SUBMIT</b-button>
            <b-button @click="nextQuiz" variant="info" class="float-right" :disabled="!answered">NEXT</b-button>
    </b-card>

    </div>
   
</template>

<script>
    import _ from "lodash"

    export default {
        props: {
            userRequest: Function,
            currentQuiz: Object,
            nextQuiz: Function,
            index: Number,
            increment: Function,
            numTotal: Number,
            numCorrect: Number
        },
        data(){
            return {
                selectedCategory: "any category",
                options: [
                    {value: "any category", text: "any category"},
                    {value: "general knowledge", text: "general knowledge"},
                    {value: "books", text: "books"},
                    {value: "music", text: "music"},
                    {value: "video games", text: "video games"},
                    {value: "science & Nature", text: "science & Nature"},
                    {value: "science & Computers", text: "science & Computers"},
                    {value: "science & Mathematics", text: "science & Mathematics"},
                    {value: "mythology", text: "mythology"},
                    {value: "sports", text: "sports"},
                    {value: "history", text: "history"},
                    {value: "art", text: "art"},
                    {value: "politics", text: "politics"},
                    {value: "celebrities", text: "celebrities"},
                    {value: "animals", text: "animals"},
                    {value: "vehicles", text: "vehicles"},
                    {value: "science & Gadgets", text: "science & Gadgets"},
                    {value: "cartoon & Animations", text: "cartoon & Animations"}
                ],
                shuffledAnswers: [],
                selectedIndex: null,
                correctIndex: null,
                answered: false,
            }
        },
        watch: {
            selectedCategory(){
                this.userRequest(this.selectedCategory)
            },
            currentQuiz: {
                immediate: true,
                handler(){
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleAnswers()
                }
            }
        },
        computed: {
            answers(){
                return this.shuffledAnswers;
            },
            percentageScore(){
                let score = 0;
                score = Math.floor((this.numCorrect / this.numTotal ) * 100);
                return score;
            }
        },
        methods: {
            shuffleAnswers(){
                let answers = [...this.currentQuiz.incorrect_answers, this.currentQuiz.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuiz.correct_answer)
            },
            getSelectedIndex(index){
                this.selectedIndex = index
            },
            answeredClass(index){
                let ansClass;
                if(!this.answered && this.selectedIndex === index){
                    ansClass = "selected"
                }else if(this.answered && this.correctIndex === index){
                    ansClass = "correct"
                }else if(this.answered && this.correctIndex !== index && this.selectedIndex === index){
                    ansClass = "incorrect"
                }
                return ansClass;
            },
            submitAnswer(){
                let isCorrect = false

                if(this.selectedIndex === this.correctIndex){
                    isCorrect = true
                }
                if(isCorrect){
                    console.log("Answer Is Correct")
                }else{
                    console.log("Wrong Answer")
                }
                this.answered = true;
                this.increment(isCorrect)
            },
        }
    }
</script>

<style scoped>
    .trivia-box{
        width: 30rem;
        margin: auto;
    }
    .list-group-item:hover{
        background: #eee;
        cursor: pointer;
    }
    .selected{
        background: lightblue;
    }
    .correct{
        background: lightgreen;
    }
    .incorrect{
        background: rgb(243, 96, 96);
    }
    .button-Div{
        display: flex;
        align-content: center;
        justify-content: center;
        margin-top: 12px;
    }
    .divCorrect, .divIncorrect{
        width: 30rem;
        margin: auto;
    }
    .percentageScore{
        font-size: 30px;
    }
</style>