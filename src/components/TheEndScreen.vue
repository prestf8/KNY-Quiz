<template>
    <div class="end-screen">
        <div class="end-screen__score-container">
            <h1 class="end-screen__score" :class="rating">{{numberCorrect}} / 10</h1>
            <button class="end-screen__restart" @click="restart">Try Again?</button>
        </div>
    </div>
</template>

<script lang="ts">
    import {Vue, Prop, Component} from 'vue-property-decorator';

    @Component
    export default class TheEndScreen extends Vue {

        @Prop() private numberCorrect!: number; 

        private rating = {
            hashira: false,
            good: false,
            mid: false,
            bottom: false,
        }

        public restart() {
            this.$emit("restart");
        }


        mounted() {
            if (this.numberCorrect == 10) {
                this.rating.hashira = true;
            } else if (this.numberCorrect >=8) {
                this.rating.good = true;
            } else if (this.numberCorrect >= 6) {
                this.rating.mid = true;
            } else {
                this.rating.bottom = true;
            }
        }  
    }

</script>

<style scoped lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Lato&display=swap');

    $lightlightgreen: #e3ffea;
    $purple: #8417ff;

    @mixin font($fontType) {
        font-family: $fontType, "sans-serif";
    }

    .hashira {
        font-weight: bold;
        color: red;
    }

    .good {
        color: darkgreen;
    }

    .mid {
        color: orange;
    }

    .bottom {
        color: gray;
    }

    .end-screen {
        background-color: white;
        border: 1px solid darkred;
        border-radius: 5px;
        box-shadow: 0px 0px 20px 5px darkred;
        width: 80%;
        max-width: 800px;
        height: 50%;
        margin: auto;

        text-align: center;
    }

    .end-screen__score-container {
        height: 100%;
        width: 100%;

        display: flex;
        flex-flow: column nowrap;
        justify-content: space-around;
    }

    .end-screen__score {
        padding: 1rem;
        @include font("Montserrat");
    }

    .end-screen__restart {
        padding: 0.7rem;
        width: 50%;
        max-width: 300px;
        margin: 0 auto;
        background-color: $purple;
        color: white;
        border: 1px solid white;
        border-radius: 5px;
        box-shadow: 0px 0px 30px 2px $purple;
        outline: none;


        @include font("Lato");
    }

    .end-screen__restart:hover {
        cursor: pointer;
    }

@media screen and (min-width: 400px) {
    .end-screen {
        height: 60%;
    }

    .end-screen__score-container {
        font-size: 1.5rem;
    }

    .end-screen__restart {
        padding: 0.7rem;
        height: 60px;
        font-size: 1.1rem;
    }
}

@media screen and (min-width: 1200px) {
    .end-screen__score-container {
        font-size: 2rem;
    }

    .end-screen__restart {
        max-width: 400px;
        font-size: 1.3rem;
    }
}



</style>