<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div class="CatButtonDiv">
      <div v-for="(question, index) in questionArray" :key="index">
        <button @click="CategoryClicked(index)">{{ question.category }}</button>
      </div>
    </div>


    <h3 v-if="questionObj">{{ questionObj.question }}</h3>

    <div class="AnsButtonDiv">
      <div v-for="(question, index) in questionObj.answers" :key="index">
        <button @click="checkAnswer(question)">{{ question }}</button>
      </div>
    </div>

    <h4>score: {{ score }}</h4>

  </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "HelloWorld",
    props: {
        msg: String
    },
    mounted() {
      this.FetchData();
    },
    methods: {
      FetchData: function(){
        this.questionObj = {};
        this.questionArray = [];
        axios.get('https://the-trivia-api.com/api/questions/')
        .then(response => {
          this.GetQuestions(response.data)
        })
        .catch(error => {
          console.log(error);
        });
      },

      GetQuestions: function(payload){
        var questionsArr = Object.values(payload);
        this.questionArray = questionsArr;
      },

      CategoryClicked: function(index){
        this.questionObj = this.questionArray[index];
        this.questionObj.answers = [this.questionObj.incorrectAnswers[0],this.questionObj.incorrectAnswers[1], this.questionObj.incorrectAnswers[2], this.questionObj.correctAnswer];
        console.log(this.questionObj);
      },

      checkAnswer: function(question){
        if(question === this.questionObj.correctAnswer){
          console.log("RIGHT");
          this.score += 1;
        }else{
          console.log("WRONG");
        }

        this.FetchData();
      }
    },
    data() {
      return{
        questionObj: {},
        questionArray: [],
        score: 0
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.CatButtonDiv {
  display: flex;
  text-align: center;
  justify-content: center;
}
.AnsButtonDiv {
  display: flex;
  text-align: center;
  justify-content: center;
}

button {
  font-size: 20px;
  text-align: center;
  background-color: #0000002b;
  border: 10px;
  border-radius: 10px;
  margin: 5px;
  color: #7592af;
  box-shadow: 5px;
  padding: 15px;
}
</style>
