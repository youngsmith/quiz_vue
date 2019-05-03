<template>
    <div>
        <b-jumbotron >
            <template slot="lead">
                {{ currentquestion.question }}
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, idx) in shuffled_answers" 
                    :key="idx"
                    @click= "select(idx)"
                    :class="list_class(idx)"
                    >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" 
            @click="check"
            :disabled="selectedIndex === null || result != null"
             href="#">submit</b-button>

            <b-button variant="success" 
            @click="next" 
            :disabled="result == null"
            href="#">next</b-button>

        </b-jumbotron>
    </div>
</template>

<script>
    export default {
        props: {
            currentquestion: Object,
            next: Function,
            correct : Function
        },
        data() {
            return {
                selectedIndex: null,
                answerIndex: null,
                shuffled_answers: [],
                result: null
            }
        },
        watch: {
            currentquestion: {
                immediate: true,
                handler : function(newVal) {
                    this.selectedIndex = null;
                    this.result = null;
                    this.shuffled_answers = [];

                    let temp = newVal.incorrect_answers.slice();
                    temp.push(newVal.correct_answer);
                    for(let i = temp.length; i > 0 ; i--){
                        let idx = Math.round((Math.random() * 10) % i);
                        if(idx == i) idx -= 1;
                        this.shuffled_answers.push(temp.splice(idx, 1)[0]);
                        if(this.shuffled_answers[this.shuffled_answers.length - 1] == newVal.correct_answer) 
                            this.answerIndex = this.shuffled_answers.length - 1;
                    }
                }
            }
        },
        methods: {
            select(index){
                this.selectedIndex = index;
            },
            check(){
                if(this.selectedIndex == this.answerIndex) {
                    this.correct();
                    this.result = 1;
                }
                else this.result = 0;    
            },
            list_class(idx){
                if(this.result == null) return (this.selectedIndex == idx ? 'selected' : '');
                else {
                    if(this.answerIndex == idx) return 'correct';
                    if(this.selectedIndex == idx) return 'incorrect';
                    return '';
                }
            }
        }
    }
</script>

<style scoped>
    .list-group{
        margin-bottom : 15px;
    }
    .list-group-item:hover{
        background: lightblue;
        cursor:pointer;
    }
    .btn {
        margin: 0 5px;
    }
    .selected {
        background-color: lightblue;
    }

    .correct {
        background-color: green;
    }

    .incorrect {
        background-color: red;
    }
</style>