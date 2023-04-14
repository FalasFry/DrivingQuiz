<template>
    <div>
      <button v-if="!isStarted" @click="StartQuiz">Starta Quiz</button>

      <div v-if="isStarted" class="QuizGame">
        
        
        <div v-if="!isCompleated" class="questionPanel">
          <h1>{{ jsonData[curQuestion].question }}</h1>
          <h2 v-if="!isCompleated">{{ "Current Question: " +(curQuestion+1) +" / " +jsonData.length }}</h2>
          <div class="questionDisplay">
              <div v-for="(answer, index) in jsonData[curQuestion].options" :key="index">
                  <button class="answerBtn" @click="CheckAnswer(answer)">{{ answer }}</button>
              </div>
          </div>
        </div>


        <div v-if="isCompleated">
          <div class="resultsPanel">
            <h1>Dina Resultat</h1>
            <h4> Rätta Svar: {{ score }} / {{ jsonData.length }} </h4>
            <h2>{{ wrongAnswers[resultDisplay].question }}</h2>
            <h2 class="correctAns">{{ "Rätt Svar: " +wrongAnswers[resultDisplay].answer }}</h2>
            <h2 class="urPick">{{ "Du Svarade: " +wrongAnswers[resultDisplay].picked }}</h2>
            <button @click="ShowResult(false)">Visa Förgående</button>
            <button @click="ShowResult(true)">Visa Nästa</button>
          </div>
        </div>

      </div>

      <button v-if="isStarted" class="endBtn" @click="EndQuiz">Avsluta Quiz</button>
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
          this.resultDisplay = 0;
          this.isCompleated = false;
          for(let i = 0; i < this.jsonData.length; i++){
            this.Shuffle(this.jsonData[i].options);
          }

          this.isStarted = true;
        },
        CheckAnswer:function(option){
            if(this.jsonData[this.curQuestion].answer === option){
                if(!this.isCompleated){
                  this.score+=1;
                }
            }else{
                this.wrongAnswers.push({
                  "question": this.jsonData[this.curQuestion].question,
                  "answer": this.jsonData[this.curQuestion].answer,
                  "picked": option,
                });
            }
            
            if(this.curQuestion < this.jsonData.length-1){
              this.curQuestion+=1;
            }
            else{
              this.isCompleated = true;
            }
        },
        ShowResult:function(upordown) {
          if(upordown){
            if(this.resultDisplay < this.wrongAnswers.length-1){
              this.resultDisplay +=1;
            }
          }
          else{
            if(this.resultDisplay > 0){
              this.resultDisplay -=1;
            }
          }
        },
        EndQuiz:function() {
          this.isStarted = false;
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
            isCompleated: false,
            resultDisplay: Number,
        }
      }
  }
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .quizGame {
    display: inline;
  }


  .endBtn {
    justify-content: center;
  }

  .questionDisplay {
    flex:1;
    display: flex;
    text-align: center;
    justify-content: center;
  }

  .questionPanel {
    display: inline-block;
    flex: 1;
    justify-content: center;
    text-align: center;
    background-color: #aaa;
    width: 900px;
    height: 240px;
    margin: 1%;
    padding: 1%;
    border-style: double;
    border-color: #252525;
    border-radius: 10%;
  }

  .resultsPanel {
    display: inline-block;
    flex: 1;
    justify-content: center;
    text-align: center;
    background-color: #aaa;
    width: 755px;
    height: 380px;
    margin: 1%;
    padding: 1%;
    border-style: double;
    border-color: #252525;
    border-radius: 10%;
  }


  .answerBtn {
    width: 210px;
    height: 60px;
    padding: 0;
  }

  .correctAns {
    color: rgb(181, 247, 181);
    background-color: rgb(14, 182, 14);
  }

  .urPick {
    color: lightsalmon;
    background-color: rgb(233, 42, 42);
  }

  @media (max-width: 768px) {
    .questionDisplay {
      display: inline;
    }

    .resultsPanel {
      width: 100%;
      height: 100%;
    }

    .questionPanel {
      width: 100%;
      height: 100%;
    }
    h1 {
      font-size: 130%;
    }
    h2 {
      font-size: 110%;
    }
  }


</style>
  