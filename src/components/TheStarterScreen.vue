<template>
  <div class="starter-screen">
    <div class="starter-screen__first">
      <h1 class="starter-screen__title">
        Demon Slayer: Kimetsu No Yaiba QUIZ
      </h1>
      <p class="starter-screen__warning">Based off MANGA</p>
    </div>
    <div class="starter-screen__second">
      <button class="starter-screen_start" @click="startQuiz">CLICK TO START</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue , Emit} from 'vue-property-decorator';

@Component
export default class TheStarterScreen extends Vue {
  @Prop() private msg!: string;

  @Emit()
  startQuiz() {
    return true;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

@import url('https://fonts.googleapis.com/css2?family=Grandstander:wght@500&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');


  .starter-screen {
    width: 100%;
    height: 100%;
    border-radius: 5px;

    position: absolute;
    top:50%;
    left:50%;
    transform: translate(-50%, -50%);

    display: flex;
    flex-flow: column wrap;
    justify-content: space-around;

    text-align: center;

    background: rgba(0,0,0,0.4);
  }
  
  .starter-screen__title {
    color: white;
    font-family: 'Grandstander', sans-serif;
    font-size: 2rem;
    padding: 1rem;
  }

  .starter-screen__warning {
    font-family: 'Open Sans', sans-serif;
    font-size: 1rem;
    color: red;
    font-weight: bold;
  }

  .starter-screen_start {
    padding: 1rem;
    width: 70%;
    max-height: 150px;
    max-width: 250px;
    font-size: 1rem;
    font-family: 'Open Sans', sans-serif;

    background: darkred;
    border: none;
    border-radius: 10px;
    color: white;
    outline: none;
  }

  .starter-screen_start:hover {
    cursor: pointer;
  }

@media screen and (min-width: 800px) {
  .starter-screen__title {
    font-size: 2.5rem;
  }
  .starter-screen__warning {
     font-size: 1.5rem;
   }

   .starter-screen_start {
     padding: 1.5rem;
     font-size: 1.5rem;
   } 
}

@media screen and (min-width: 1000px) {
    .starter-screen__title {
    font-size: 3.5rem;
  }
  .starter-screen__warning {
     font-size: 2rem;
   }
}

@media screen and (min-width: 1500px) {
  .starter-screen__title  {
    font-size: 4.5rem;
  }

  .starter-screen__warning {
    font-size: 3rem;
  }

  .starter-screen_start {
    padding: 1.5rem;
    font-size: 1.5rem;
    max-height: 170px;
    max-width: 300px;
   } 
}

</style>
