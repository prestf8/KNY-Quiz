<template>
  <div id="app">
    <div class="app__background"></div>
    <img class="app__logo" alt="Logo" src="./assets/logo.png">

    
    <TheStarterScreen v-if="stages.start" msg="Welcome to Your Vue.js + TypeScript App" @start-quiz="startQuiz" />
    
    <section class="app__questions-section" v-else-if="stages.running">
      <QuestionTemplate v-for="question in questionFilter" :key="question.id" :question="question" :unAnswered="unAnswered" @next="next" @back="back" @radio-onchange="updateUserAnswer"></QuestionTemplate>
    </section>

    <TheEndScreen v-if="stages.end" :numberCorrect="numberOfCorrect" @restart="restartQuiz"></TheEndScreen>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import uniqueId from "lodash/uniqueId";

import TheStarterScreen from './components/TheStarterScreen.vue';
import QuestionTemplate from './components/QuestionTemplate.vue';
import TheEndScreen from './components/TheEndScreen.vue';

interface StageStructure {
  start: boolean;
  running: boolean;
  end: boolean;
}

interface Question {
    // [index: number]: { 
      id: string; 
      label: string; 
      answerChoices: string[]; 
      answer: string; 
      userAnswer: string;
  // };
}

@Component({
  components: {
    TheStarterScreen,
    QuestionTemplate,
    TheEndScreen,
  },
})
export default class App extends Vue {
  private stages: StageStructure = {
    start: true,
    running: false,
    end: false
  }

  private questionData: Array<Question> = [
  {
    id: uniqueId('question-'),
    label: "Which Demon does Kaigaku replace?",
    answerChoices: ["Rui", "Daki", "Kokushibo", "Nezuko"],
    answer: "Daki",
    userAnswer:""
  },
  {
    id: uniqueId('question-'),
    label: "Who is the brother of Yoriichi?",
    answerChoices: ["Tokito", "Akaza", "Kokushibo", "Rengoku"],
    answer: "Kokushibo",
    userAnswer:""
  },
  {
    id: uniqueId('question-'),
    label: "Which episode was the fight with Zenitsu vs Spider Demon?",
    answerChoices: ["Episode 5", "Episode 10", "Episode 19", "Episode 17"],
    answer: "Episode 17",
    userAnswer:"",
  },
  {
    id: uniqueId('question-'),
    label: "What is Inosuke Hashibira's Combat Style (Type of Breathing)",
    answerChoices: ["Sun Breathing", "Thunder Breathing", "Water Breathing", "Beast Breathing"],
    answer: "Beast Breathing",
    userAnswer:""
  },
  {
    id: uniqueId('question-'),
    label: "Which Pillar dies in the Mugen Train Arc?",
    answerChoices: ["Kyojuro Rengoku", "Tengen Uzui", "Shinobu Kocho", "Gyomei Himejima"],
    answer: "Kyojuro Rengoku",
    userAnswer:"",

  },
  {
    id: uniqueId('question-'),
    label: "Who is the Wind Hashira?",
    answerChoices: ["Gyomei Himejima", "Mitsuri Kanroji", "Sanemi Shinazugawa", "Giyu Tomioka"],
    answer: "Sanemi Shinazugawa",
    userAnswer:""

  },
    {
    id: uniqueId('question-'),
    label: "What is the \"Hinokami Kagura\" (In Brief Terms)?",
    answerChoices: ["Sun Breathing Style", "Dance of the Fire God", "An Antidote", "A Flame Related Combat Ability"],
    answer: "Dance of the Fire God",
    userAnswer:""
  },
  {
    id: uniqueId('question-'),
    label: "Which of these 4 survived the Infinity Castle Arc?",
    answerChoices: ["Mitsuri Kanroji", "Obanai Iguro", "Shinobu Kocho", "Giyu Tomioka"],
    answer: "Giyu Tomioka",
    userAnswer:""

  },
    {
    id: uniqueId('question-'),
    label: "Which Demon does Tanjiro fight in the Mugen Train Arc?",
    answerChoices: ["Nakime", "Hantengu", "Doma", "Enmu"],
    answer: "Enmu",
    userAnswer:""

  },
  {
    id: uniqueId('question-'),
    label: "Who was Tanjiro's trainer?",
    answerChoices: ["Sakonji Urokodaki", "Tengen Uzui", "Genya Shinazugawa", "Kagaya Ubuyashiki"],
    answer: "Sakonji Urokodaki",
    userAnswer:""
  }
];

  private current = 0;

  private numberOfCorrect=0;

  private unAnswered = true;

  public startQuiz(): void {
    this.stages.start = false;
    this.stages.running = true;
  }

  public next(): void {    
    this.current++;

        // console.log(this.questionData[this.current].userAnswer);


    if (this.current == this.questionData.length) {
      if (this.questionData.some(question => question.userAnswer == "")) {
        this.current = this.questionData.length-1;
        this.unAnswered = false;
      } else {
        this.endQuiz();
      }
    }
  }

  public back(): void {

    this.current--;

    // if (this.current > 0) {
    //   console.log(this.questionData[this.current].userAnswer);
    // }

    if(this.current < 0) {
      this.current=0;
    }
  }

  public updateUserAnswer(markedAnswer: string) {
    this.questionData[this.current].userAnswer = markedAnswer;
  }

  public endQuiz() {
    // if (this.questionData.some(question => question.userAnswer == "")) {
      // console.log("Unanswered");
      // this.unAnswered = true;
      // return;
    // }
    this.stages.running = false;
    this.stages.end = true;
    // let correct: string[] = this.questionData.filter(question => question.userAnswer == question.answer);

    const correct: Array<object> = (this.questionData.filter(question => 
    {
      const userAnswer: string = question.userAnswer;
      const answer: string = question.answer;

      return userAnswer == answer;
    }));

    this.numberOfCorrect = correct.length;
  }

  public restartQuiz() {
    this.stages.running=true;
    this.stages.end = false;
    this.current = 0;
    this.questionData.forEach(question => question.userAnswer = "");
    this.unAnswered = true;
  }

  get questionFilter(): object {
    return this.questionData.filter(question => 
    question.id == this.questionData[this.current].id)
  }
}
</script>

<style lang="scss">

@mixin fontFamily($font-family) {
  font-family: $font-family, sans-serif;
}

* {
  margin:0;
  padding:0;
  box-sizing: border-box;
}

#app {
  @include fontFamily('Avenir');
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  width: 100%;
  height: 100vh;
  overflow: hidden;
  display: flex;
}

.app__background {
  position: absolute;
  top:0;
  left:0;

  max-height: 100%;
  width: 100%;
  height: 100vh;
  opacity: 0.8;


  background-image: url("./assets/small-bg.png");
  background-size: cover;

  z-index: -999;
}

.app__questions-section {
  width: 90%;
  height: 70%;

  max-width: 600px;
  max-height: 400px;

  margin: auto;
}

.app__logo {
  position: absolute;
  bottom:0;
  right:0;
  margin: 1rem;
  height: 80px;
}

@media screen and (min-width: 800px) {
  .app__logo {
    height: 120px;
  }

  .app__background {
    background-image: url("./assets/medium-bg.jpg");
  }
}

@media screen and (min-width: 1200px) {
  .app__questions-section {
    min-height: 500px;
    min-width: 700px;
  }
}

@media screen and (min-width: 1500px) {

  .app__logo {
    height: 150px;
  }

  .app__background {
    background-image: url("./assets/large-bg.jpg");
  }
}



</style>
