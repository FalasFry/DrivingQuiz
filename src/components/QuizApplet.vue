<template>
    <div>
      <button v-if="!isStarted" @click="StartQuiz">Start Quiz</button>
      <button v-if="isStarted">End Quiz</button>

      <div v-if="isStarted" class="QuizGame">
        <h2>Welcome To The Quiz</h2>
        
        <h4> Rätta Svar: {{ score }} / {{ jsonData.length }} </h4>
        <h3>{{ jsonData[curQuestion].question }}</h3>


        <div class="questionDisplay">
            <div v-for="(answer, index) in jsonData[curQuestion].options" :key="index">
                <button @click="CheckAnswer(answer)">{{ answer }}</button>
            </div>
        </div>
      </div>
    </div>
</template>
  
<script>
  import jsonFile from '../assets/questions.json'
  
  export default {
      name: "QuizApp",
      methods: {
        StartQuiz:function(){
          this.jsonData = jsonFile.questions;
          this.curQuestion = 0;
          this.score = 0;
          this.Shuffle(this.jsonData);
          this.wrongAnswers = [];

          this.isStarted = true;
        },
        CheckAnswer:function(option){
            if(this.jsonData[this.curQuestion].answer === option){
                console.log('CORRECT');
                this.score+=1;
            }else{
                console.log('WRONG');
            }
            
            if(this.curQuestion < this.jsonData.length-1){
              this.curQuestion+=1;
            }
        },
        Shuffle:function(array) {
          for (var i = array.length - 1; i > 0; i--) {
            var j = Math.floor(Math.random() * (i + 1));
            var temp = array[i];
            array[i] = array[j];
            array[j] = temp;
          }
        }

      },
      data() {
        return{
            jsonData: Object,
            curQuestion: Number,
            score: Number,
            isStarted: false,
            wrongAnswers: Array,
        }
      }
  }
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .questionDisplay {
    flex:1;
    display: flex;
    text-align: center;
    justify-content: center;
  }

  @media (max-width: 768px) {
    .questionDisplay {
      display: inline;
    }
  }
</style>
  