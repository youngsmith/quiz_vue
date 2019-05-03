<template>
  <div id="app">
    <Header
      :score="score"
      :numQuestion="questions.length"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <question 
            v-if="questions.length"
            :currentquestion="questions[index]"
            :next="next"
            :correct="correct"
          />
          <h1 class="loading" v-else> loading... </h1>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/header.vue'
import question from './components/question.vue'


export default {
  name: 'app',
  components: {
    Header,
    question
  },
  data() {
    return {
      questions: [],
      index: 0,
      score: 0
    }
  },
  methods: {
    next() {
      if(this.index < this.questions.length - 1) this.index++;
    },
    correct() {
      this.score += 1;
    }
  },
  created(){
      fetch('https://opentdb.com/api.php?amount=10', {
          method: 'get'
      })
      .then((result)=>{
          return result.json();
      })
      .then((data)=>{
          this.questions = data.results;
      });
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.loading{
  margin-top: 200px;
}

</style>
