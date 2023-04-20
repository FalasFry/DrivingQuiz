<template>
    <div>
      <header>
        <h1> Header </h1>
      </header>
      <h1 v-if="!isStarted">Lär Dig Teori!</h1>
      <button v-if="!isStarted" @click="StartQuiz">Starta Quiz</button>

      <div v-if="isStarted" class="QuizGame">
        
        <div v-if="!isCompleated" class="questionPanel">
          <h1>{{ jsonData[curQuestion].question }}</h1>
          <h2 v-if="!isCompleated">{{ "Current Question: " +(curQuestion+1) +" / " +jsonData.length }}</h2>
          <div class="questionDisplay">
              <div v-for="(answer, index) in jsonData[curQuestion].options" :key="index">
                  <label :class="[{'selected': selectedAnswer === answer}, 'answerLabel']">
                    <input type="radio" class="answerInput" :value="answer" v-model="selectedAnswer">
                    {{ answer }}
                  </label>
              </div>
          </div>
          <button @click="CheckAnswer(selectedAnswer)">Next</button>
        </div>
        
        <div v-if="isCompleated">
          <div v-if="wrongAnswers.length > 0" class="resultsPanel">
            <h1>Dina Resultat</h1>
            <h4> Rätta Svar: {{ score }} / {{ jsonData.length }} </h4>
            <h2>{{ wrongAnswers[resultDisplay].question }}</h2>
            <h2 class="correctAns">{{ "Rätt Svar: " +wrongAnswers[resultDisplay].answer }}</h2>
            <h2 class="urPick">{{ "Du Svarade: " +wrongAnswers[resultDisplay].picked }}</h2>
            <button @click="ShowResult(false)">Visa Förgående</button>
            <button @click="ShowResult(true)">Visa Nästa</button>
          </div>
          <div v-else>
            <h1>Grattis Alla Rätt!</h1>
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
          this.jsonData.splice(65);
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
          this.selectedAnswer = null;
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
            selectedAnswer: null,
        }
      },
  }
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .quizGame {
    display: inline;
  }

  .endBtn {
    justify-content: center;
  }

  .questionDisplay {
    display: grid;
    text-align: center;
    align-items: center;
    justify-content: center;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, 1fr);
    line-height: 200%;
  }

  .questionPanel {
    display: inline-block;
    flex: 1;
    justify-content: center;
    text-align: center;
    background-color: whitesmoke;
    width: 900px;
    height: 100%;
    border-style: double;
    border-color: #252525;
    border-radius: 10%;
    padding: 1%;
  }

  .resultsPanel {
    display: inline-block;
    flex: 1;
    justify-content: center;
    text-align: center;
    background-color: whitesmoke;
    width: 755px;
    height: 380px;
    margin: 1%;
    padding: 1%;
    border-style: double;
    border-color: #252525;
    border-radius: 10%;
  }

  .answerLabel {
    display: inline-block;
    margin: 10px;
    padding: 10px;
    border: 2px solid black;
    border-radius: 10px;
    cursor: pointer;
    width: 70%;
    height: 70px;
    justify-content: center;
    text-align: center;
    align-items: center;
  }
  .answerLabel.selected {
    background-color: rgb(199, 199, 199);
    border-color: rgb(102, 102, 102);
  }
  .answerInput {
    display: none;
  }

  .correctAns {
    color: rgb(181, 247, 181);
    background-color: rgb(14, 182, 14);
  }

  .urPick {
    color: rgb(247, 181, 181);
    background-color: rgb(182, 14, 14);
  }

  @media (max-width: 950px) {
    .questionDisplay {
      line-height: 150%;
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

  @media (max-height: 450px) {
    .questionDisplay {
      line-height: 150%;
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
  