<template>
    <div class="container">
    <div class="card back">
    <div class="card-header">
        Congratulations!
    </div>
    <div class="card-body text-center">
    <p>That is the correct Answer!!</p>
    <input @click="nextQuestion" type="button"  class="btn btn-primary" value="Next Question!">
  
      </div>
    </div>
    </div>
</template>

<script>
import {eventBus} from '../main.js';
    export default {
        methods:{
            nextQuestion(){
                eventBus.$emit('nextQuestionEvent',true);
            }
        }
    }
</script>


<style scoped>
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