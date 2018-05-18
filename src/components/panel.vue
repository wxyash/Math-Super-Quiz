<template>
<div class="container">
<div class="card front">
  <div class="card-header">
    Question {{questionNumber}}
  </div>
  <div class="card-body">
    <h5 class="card-title">Question</h5>
    <p class="card-text">What should be the result of <span style="margin:5px;padding:5px; font-weight:500; background-color:#f9efef;" >{{qnaArray[0]}}</span>  ?</p>
    <input @click="checkAnswer" type="button" v-model="btn[0]" class="animated btn btn-primary float-left">
    <input @click="checkAnswer" type="button" v-model="btn[1]" class="animated btn btn-primary float-right"><br><br>
    <input @click="checkAnswer" type="button" v-model="btn[2]" class="animated btn btn-primary float-left">
    <input @click="checkAnswer" type="button" v-model="btn[3]" class="animated btn btn-primary float-right">
  </div>
</div>
</div>    
</template>


<script>
    import {
        eventBus
    } from '../main.js';
    export default {
        data() {
            return {
                btn: [0, 0, 0, 0],
            }
        },
        props: ['qnaArray','questionNumber'],
        methods: {
            shuffleAnswers() {
                for (var i = 0; i < this.btn.length ; i++) {
                    var rand = Math.floor(Math.random() * Math.floor(3));

                    if (rand % 2 == 0) {
                        this.btn[i] = Math.floor(Math.random() * Math.floor(this.qnaArray[1]));
                    } else {
                        this.btn[i] = Math.floor(Math.random() * Math.floor(this.qnaArray[1])) + Math.floor(Math.random() * Math.floor(this.qnaArray[1]));
                    }

                }
                this.btn[Math.floor(Math.random() * Math.floor(4))] = this.qnaArray[1];
                return;
            },
            checkAnswer(data) {
                if (data.target.value == this.qnaArray[1]) {
                    data.target.classList.remove('btn-primary');
                    data.target.classList.add('btn-success');
                    setTimeout(function() {
                        eventBus.$emit('correctAnswerEvent',data);
                    },400)
                    return;
                }
                data.target.classList.remove('btn-primary');
                data.target.classList.add('jello');
                data.target.classList.add('btn-danger');
                setTimeout(function() {
                    data.target.classList.remove('btn-danger');
                    data.target.classList.remove('jello');
                    data.target.classList.add('btn-primary');
                }, 2000)
            },

        },
        created() {
            this.shuffleAnswers();
        }
    }

</script>


<style scoped="true">
    .card {
    width:70%;
    margin: auto;
    }

    .btn,
    .card-title,
    .card-text {
        margin-left: 30px;
        margin-right: 30px;
    }

    .btn {
        min-width: 80px;
    }
    

</style>
