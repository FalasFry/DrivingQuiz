<template>
    <div>
      <h1 v-if="!isStarted">Lär Dig Teori!</h1>
      <button v-if="!isStarted" @click="StartQuiz">Starta Quiz</button>

      <div v-if="isStarted" class="QuizGame">
        
        <div v-if="!isCompleated" class="questionPanel">

          <div  class="image-container">
            <div class="imageDisplay">
              <img v-if="jsonData[curQuestion].src" :src="jsonData[curQuestion].src" class="fit-image">
            </div>
          </div>

          <h1>{{ jsonData[curQuestion].question }}</h1>

          <h2 v-if="!isCompleated">{{ "Fråga nummer: " +(curQuestion+1) +" / " +jsonData.length }}</h2>
          <div class="questionDisplay">
              <div v-for="(answer, index) in jsonData[curQuestion].options" :key="index">
                  <label :class="[{'selected': selectedAnswer === answer}, 'answerLabel']">
                    <input type="radio" class="answerInput" :value="answer" v-model="selectedAnswer">
                    {{ answer }}
                  </label>
              </div>
          </div>
          <button v-if="selectedAnswer" @click="CheckAnswer(selectedAnswer)">Nästa Fråga</button>
          <button v-else  class="unclickable">Nästa Fråga</button>
        </div>
        
        <div v-if="isCompleated">
          <div v-if="wrongAnswers.length > 0" class="resultsPanel">
            <h1>Dina Resultat</h1>

            <div  class="image-container">
              <div class="imageDisplay">
                <img v-if="wrongAnswers[resultDisplay].src" :src="wrongAnswers[resultDisplay].src" class="fit-image">
              </div>
            </div>

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
          this.selectedAnswer = null;
          this.jsonData = jsonFile.questions;
          this.curQuestion = 0;
          this.score = 0;
          this.jsonData.splice(65);
          this.Shuffle(this.jsonData);
          this.wrongAnswers = [];
          this.images = [];
          this.resultDisplay = 0;
          this.isCompleated = false;
          for(let i = 0; i < this.jsonData.length; i++){
            this.Shuffle(this.jsonData[i].options);
          }
          this.ImportImages();
          for(let i = 0; i < this.images.length; i++){
            
            for(let j = 0; j < this.jsonData.length;j++){
              if(this.images[i].path === this.jsonData[j].img){
                this.jsonData[j].src = this.images[i].src;
              }
              else if(this.images[i].builtPath === this.jsonData[j].img){
                this.jsonData[j].src = this.images[i].src;
              }
            }
          }
          this.isStarted = true;
        },
        CheckAnswer:function(option) {
          if(this.jsonData[this.curQuestion].answer === option){
              if(!this.isCompleated){
                this.score+=1;
              }
          }else{
              this.wrongAnswers.push({
                "question": this.jsonData[this.curQuestion].question,
                "answer": this.jsonData[this.curQuestion].answer,
                "picked": option,
                "src": this.jsonData[this.curQuestion].src,
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
        ImportImages:function() {
          const files = require.context('@/assets/images', false, /\.jpg$/);
          files.keys().forEach(key => {
            let path = files(key).split('/')[2].split('.')[0];
            let builtPath = files(key).split('/')[3].split('.')[0];
            //let builtPath = 0;
            this.images.push({
              "src": files(key),
              "path": path,
              "builtPath": builtPath,
            });
          });
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
            images: [],
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
    text-align: center;
    background-color: whitesmoke;
    width: 900px;
    height: 100%;
    border-style: double;
    border-color: #252525;
    border-radius: 10%;
    padding: 1%;
  }

  .unclickable {
    opacity: 40%;
  }

  .image-container {
    display: flex;
    justify-content: center;
  }

  .imageDisplay {
    background-color: whitesmoke;
    padding: 2%;
    width: 800px;
    height: 450px;
    border-radius: 10%;
  }

  .fit-image {
    height: 100%;
    width: 100%;
    display: block;
    margin-left: auto;
    margin-right: auto;
    border: 1%;
    border-radius: 10%;
  }

  .resultsPanel {
    display: inline-block;
    flex: 1;
    justify-content: center;
    text-align: center;
    background-color: whitesmoke;
    width: 755px;
    height: 100%;
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
    border: 2px solid #252525;
    border-radius: 10px;
    cursor: pointer;
    width: 70%;
    height: 70px;
    text-align: center;
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

  @media (max-width: 450px) {
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

    .image-container {
      padding-left: 3%;
      padding-right: 3%;
    }
    .imageDisplay {
      padding: 2px;
      width: 100%;
      height: calc(100vw / 1.7777778)
    }

    .answerLabel {
      width: 70%;
      height: calc(70vw / 3);
      line-height: 100%;
      text-align: center;
      padding-bottom: -50vh;
    }

    h1 {
      font-size: 4.7vw;
      padding-left: 5%;
      padding-right: 5%;
    }
    h2 {
      font-size: 4.2vw;
    }
  }

  @media (max-height: 820px) {
    .questionDisplay {
      line-height: 150%;
    }

    .questionPanel {
      width: 100%;
    }

    .resultsPanel {
      width: 100%;
    }

    .imageDisplay {
      padding: 2px;
      height: calc(30vh / 0.5625);
      width: 70%;
    }

    h1 {
      font-size: 6vh;
      padding-left: 5%;
      padding-right: 5%;
    }
    h2 {
      font-size: 5vh;
    }
  }
</style>
  