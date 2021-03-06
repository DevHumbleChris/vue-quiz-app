<template>
    <b-card
        title="Card Title"
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
        <b-card-text>
        Some quick example text to build on the card title and make up the bulk of the card's content.
        </b-card-text>

        <b-button href="#" variant="primary">SUBMIT</b-button>
        <b-button href="#" variant="info" class="float-right">NEXT</b-button>
  </b-card>
   
</template>

<script>
    export default {
        props: {
            userRequest: Function
        },
        data(){
            return {
                questions: [],
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
                ]
            }
        },
        watch: {
            selectedCategory(){
                this.userRequest(this.selectedCategory)
            }
        }
    }
</script>

<style scoped>
    .trivia-box{
        width: 30rem;
        margin: auto;
    }
</style>