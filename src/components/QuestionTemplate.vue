<template>
    <div class="question-template">
        <div class="question-template__question-container">
            <h6 class="question-template__question">{{label}}</h6>
        </div>


        <form class="question-template__form">
            <label v-for="(item, index) in 4" :key="index">
                <input type="radio" :name="id" :value="answerChoices[index]" :id="`input-${index+1}`" :ref="`input${index+1}`" v-model="markedAnswer" @change="radioOnChange"/>
                {{answerChoices[index]}}
            </label> 

            <!-- Redundant Code -->

            <!-- <label for="input-1">
                <input id="input-1" type="radio" :name="id" :value="answerChoices[0]" ref="input1" v-model="markedAnswer" @change="radioOnChange" >
                {{answerChoices[0]}}
            </label>

            <label for="input-2">
                <input id="input-2" type="radio" :name="id" :value="answerChoices[1]" ref="input2" v-model="markedAnswer" @change="radioOnChange">
                {{answerChoices[1]}}
            </label>

            <label for="input-3">
                <input id="input-3" type="radio" :name="id" :value="answerChoices[2]" ref="input3" v-model="markedAnswer" @change="radioOnChange">
                {{answerChoices[2]}}
            </label>

            <label for="input-4">
                <input id="input-4" type="radio" :name="id" :value="answerChoices[3]" ref="input4" v-model="markedAnswer" @change="radioOnChange">
                {{answerChoices[3]}}
            </label> -->

        </form>

        <div class="question__bottom">
            <button class="question__back" @click="back">Back</button>
            <span v-show="unAnswered==false" class="question__warning">You have not answered all of the questions!</span>
            <button class="question__next" @click="next">Next</button>
        </div>
    </div>   
</template>

<script lang="ts">
    import { Component, Emit, Prop, Vue } from 'vue-property-decorator';

    interface LabelObject {
        [key: string]: any;
    }

    @Component
    export default class QuestionTemplate extends Vue {

        @Prop() private question!: LabelObject;
        @Prop() private unAnswered!: boolean;

        $refs!: {
            input1: HTMLInputElement;
            input2: HTMLInputElement;
            input3: HTMLInputElement;
            input4: HTMLInputElement;
        }

        private id = this.question.id;
        private label = this.question.label;
        private answerChoices = this.question.answerChoices; 
        private answer = this.question.answer; 

        private markedAnswer = "";

        mounted() {
            this.markedAnswer = this.question.userAnswer;
        }



        public next(){
            this.$emit("next");
        }

        public back() {
            this.$emit("back");
        }

        @Emit("radio-onchange")
        public radioOnChange(event: Event) {
            return this.markedAnswer;
        }
    }



</script>

<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Lato&display=swap');

    $lightlightgray: #e4e3e3;
    $lightgray: #e0e0e0;
    $darkaqua: #009db8;
    $darkred: #820000;
    $lightred: #ff4f4f;

    @mixin display($direction, $justify, $align) {
        display: flex;
        flex-flow: $direction nowrap;
        justify-content: $justify;
        align-items: $align;
    }

    @mixin font($font) {
        font-family: $font, 'san-serif';
    }

    %button {
        padding: 0.8rem;
        margin: 0.5rem;
        font-size: 0.7rem;
        border: 1px solid white;
        border-radius: 3px;
        outline: none;
        color: $lightlightgray;

        @include font("Open Sans");
    }

    %button:hover {
        cursor: pointer;
        // background-color: $lightgray;
    }

    .question-template {
        height: 100%;
        width: 100%;
        background-color: $lightlightgray;
        border-radius: 5px;
        box-shadow: 0px 0px 4rem 3px $lightred;
    
        position: relative;
    }

    .question-template__question {
        font-size: 0.9rem;
    }

    .question-template__question-container {
        font-size: 2rem;
        padding: 1rem;
        text-align: center;
        background: $darkred;
        color: white;
        
        @include font("Open Sans");
        
    }


    .question-template__form {
        display: grid;
        grid-template-columns: 50% 50%;

        text-align: center;
        // background: yellow;
        height: 200px;

        font-size: 1.1rem;
    }

    .question-template__form > label {
        width: 50%;
        place-self: center;
        font-size: 0.8rem;

        @include font("Source Sans Pro");
    }

    .question-template__form > label:hover {
        cursor: pointer;
    }

    input[type="radio"] {
        transform: scale(1.1);
    }

    input[type="radio"]:hover {
        cursor: pointer;
    }

    .question__bottom {
        text-align: center;
        @include display(row, space-around, center);

        width: 100%;
        position: absolute;
        bottom: 0;
    }

    .question__next {
        @extend %button;
        background-color: $darkaqua;
        
        box-shadow: 0px 0px 15px 1px $darkaqua;

    }

    .question__warning {
        color: red;
        font-size: 0.8rem;

        @include font("Lato");
    }   

    .question__back {
        @extend %button;
        background-color: $darkred;

        box-shadow: 0px 0px 15px 1px $darkred;

    }


    @media screen and (min-width: 400px) {
        .question-template__question {
            font-size: 1.2rem;
        }

        .question-template__form > label {
            font-size: 0.9rem;
        }

        %button {
            padding: 1rem;
            width: 100px;
            font-size: 0.9rem;
        }
    }

    @media screen and (min-width: 1200px) {
        .question-template__question {
            font-size: 1.4rem;
        }

        .question-template__form {
            height: 300px;
        }

         .question-template__form > label {
             font-size: 1.2rem;
         }

        .question__warning {
            font-size: 0.9rem;
        }

    
        
    }


</style>