<template>
    <div class="container-fluid">
    <br><hr>
    <div class="row"><div class="col-md-6 offset-md-3 col-sm-12 text-center"><h2>Math Super Quiz</h2></div></div>
    <hr>
    <br>
    <transition mode="out-in" name="fade">
        <component
         :qnaArray="questionAnswer" 
         :questionNumber="questionNumber" 
         :is="currentComponent"></component>
    </transition>
     
   
<!--
    <app-panel  :qnaArray="questionAnswer" v-if="isTrue==false" ></app-panel>
    <app-panelFlipped  v-if="isTrue==true"></app-panelFlipped>
-->
    </div>
    
</template>

<script>
    import panel from './components/panel.vue'
    import panelFlipped from './components/panelFlipped.vue'
    import {eventBus} from './main.js'

    export default {
        data() {
            return {
                questionAnswer: ['', null],
                isTrue: false,
                questionNumber:0,
                currentComponent:'app-panel'
            }
        },
        components: {
            'app-panel': panel,
            'app-panelFlipped': panelFlipped
        },
        methods: {
            generateQuestion() {
                this.currentComponent= 'app-panel'
                this.isTrue=false;
                this.questionNumber++;
                var expression1 = Math.floor(Math.random() * Math.random() + (45 * Math.random()));
                var expression2 = Math.floor(Math.random() * Math.random() + (44 * Math.random()));

                var operatorArray = [' + ', ' - ', ' * ', ' / '];

                var choosenOperator = Math.floor(Math.random() * Math.floor(4));
                this.questionAnswer[0] = ' ' + expression1 + ' ' + operatorArray[choosenOperator] + ' ' + expression2;

                if (choosenOperator == 0) {
                    this.questionAnswer[1] = Math.round(expression1 + expression2);
                } else if (choosenOperator == 1) {
                    this.questionAnswer[1] = Math.round(expression1 - expression2);
                } else if (choosenOperator == 2) {
                    this.questionAnswer[1] = Math.round(expression1 * expression2);
                } else {
                    this.questionAnswer[1] = Math.round(expression1 / expression2);
                }

                return;
            }
        },
        created() {
            this.generateQuestion();
            eventBus.$on('correctAnswerEvent', (data) => {
                console.log(data);
                this.isTrue=true;
                this.currentComponent = 'app-panelFlipped';
            });
            eventBus.$on('nextQuestionEvent',()=>{
                this.generateQuestion();
            })
        }
    }
</script>

<style>
    .fade-enter{}
    .fade-enter-active{animation: flip-in 0.5s ease-out forwards}
    .fade-leave{}
    .fade-leave-active{animation: flip-out 0.5s ease-out forwards}
    
    @keyframes flip-out{
        from{transform: rotateY(0deg)}
        to{transform: rotateY(90deg)}
    }
    
    @keyframes flip-in {
        from{transform: rotateY(90deg)}
        to{transform: rotateY(0deg)}
    }
    
</style>