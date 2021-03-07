<template>
    <b-card
        :title="currentQuiz.category"
        tag="article"
        style="max-width: 30rem; margin: auto; margin-top: 50px;"
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
            <b-list-group-item v-for="(answer, index) in answers" :key="index" v-html="answer" @click="getSelectedIndex(index)" :class="[selectedIndex === index ? 'selected': '']"></b-list-group-item>
        </b-list-group>
        <b-button href="#" variant="primary">SUBMIT</b-button>
        <b-button @click="nextQuiz" variant="info" class="float-right">NEXT</b-button>
  </b-card>
   
</template>

<script>
    import _ from "lodash"

    export default {
        props: {
            userRequest: Function,
            currentQuiz: Object,
            nextQuiz: Function,
            index: Number
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
                selectedIndex: null
            }
        },
        watch: {
            selectedCategory(){
                this.userRequest(this.selectedCategory)
            },
            currentQuiz: {
                immediate: true,
                handler(){
                    this.shuffleAnswers()
                    this.selectedIndex = null
                }
            }
        },
        computed: {
            answers(){
                return this.shuffledAnswers;
            },
        },
        methods: {
            shuffleAnswers(){
                let answers = [...this.currentQuiz.incorrect_answers, this.currentQuiz.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
            },
            getSelectedIndex(index){
                this.selectedIndex = index
                console.log(index)
            }
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
</style>