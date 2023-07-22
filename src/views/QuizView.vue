<script setup>
import Question from "@/components/Question.vue";
import QuizHeader from "@/components/QuizHeader.vue";
import {useRoute} from "vue-router";
import {computed, ref} from "vue";
import quizes from '../data/quizes.json'
import Result from "@/components/Result.vue";

  const route = useRoute()
  const quizId = parseInt(route.params.id)
  const quiz = quizes.find(q=>q.id === quizId)
  const currentQuestionIndex = ref(0)
const numberOfCorrectAnswers = ref(0);
  const showResults = ref(false)

const questionStatus = computed(()=>{
    return `${currentQuestionIndex.value}/${quiz.questions.length}`
})

const barPercentage = computed(()=>{
    return `${(currentQuestionIndex.value/quiz.questions.length)*100}%`
})

const onOptionSelected = (isCorrect)=>{
    if(isCorrect){
        numberOfCorrectAnswers.value++
    }

    if(quiz.questions.length === currentQuestionIndex.value+1){
        showResults.value = true
    }

    currentQuestionIndex.value++
}


</script>

<template>

  <div>
      <QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage"/>
      <div>
          <Question
                  v-if="!showResults"
                  :question="quiz.questions[currentQuestionIndex]"
                    @selectOption="onOptionSelected"
          />
          <Result :questionsLength="quiz.questions.length" :numberOfCorrectAnswers="numberOfCorrectAnswers"  v-else/>
      </div>

  </div>
</template>