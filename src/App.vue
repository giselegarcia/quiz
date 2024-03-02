<template>
  <div  id="app">
          <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />
          <template v-if="this.question">
              <div>
                <h1 v-html="this.question"></h1>
                  <template v-for="(answer, index) in this.answers"   :key="index">
                    <div class="retorno">
                      <input 
                        :disabled="this.answerSubmitted"
                        type="radio"
                        name="options"
                        :value="answer"
                        v-model="this.chosen_answer">
                      <label for="" v-html="answer"></label>
                    </div>
                  </template>
                <button 
                v-if="!this.answerSubmitted"
                v-on:click="this.submitAnswer()"
                class="send" 
                type="button"
                >Enviar</button>
              </div>
              <section v-if="this.answerSubmitted" class="result">
                <h4 v-if="this.chosen_answer == this.correct_answer"
                v-html="'&#9989; Parabéns você acertou!'"
                ></h4>
                <h4 v-else  
                v-html=" '&#10060; Sinto muito você escolheu uma resposta errada. A resposta correcta é ' + this.correct_answer">
                </h4>
                <button @click="this.getNewQuestion()" class="send" type="button">Próxima questão</button>
              </section>
          </template>
  </div>
</template> 
<script>

import ScoreBoard from '@/components/ScoreBoard.vue'
export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data(){
    return{
      question:  undefined,
      incorrect_answers: undefined,
      correct_answer: undefined,
      chosen_answer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0
    }
  },
  computed:{
    answers(){
      var answers = JSON.parse(JSON.stringify(this.incorrect_answers));
      answers.splice(Math.round(Math.random()*4), 0, this.correct_answer);
      return answers
    }
  },
  methods:{
    submitAnswer(){
      if(!this.chosen_answer){
        alert("Escolha uma das opções")
      }else{
        this.answerSubmitted = true
          if(this.chosen_answer == this.correct_answer){
            this.winCount++;
          }else{
            this.loseCount++;
          }
      }
    },
    getNewQuestion(){
      this.answerSubmitted = false;
      this.chosen_answer = undefined;
      this.question = undefined;
      this.axios
        .get('https://opentdb.com/api.php?amount=1&category=18&difficulty=medium')
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrect_answers = response.data.results[0].incorrect_answers;
          this.correct_answer = response.data.results[0].correct_answer;

        //console.log(response.data.results[0])
      })
    }
  },
  created(){
    this.getNewQuestion()
  }
}

</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  label{
    margin-left: 8px;
  }
  .retorno{
    display: flex;
    justify-content: left;
    margin: 8px 0;
  }
  button{
    border-radius: 8px;
    background: #145ba2;
    padding: 12px 16px;
    min-width: 220px;
    border: none;
    color: rgb(255, 255, 255);
    font-size: 18px;
    margin: 16px 0;
  }
  h1{
    max-width: 800px;
    font-size: 22px;
  }
  
}
</style>
