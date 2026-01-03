<template>
  <div class="lesson">
    <div class="lesson__wrapper">
      <h1 class="lesson__question">
        {{ currentQuestion.question }}
      </h1>

      <p class="lesson__score">
        Твой результат: {{ userScore }} из {{ Questions.length }}
      </p>

      <p class="lesson__round">
        Вопрос {{ currentQuestionIndex + 1 }}
      </p>

      <div class="lesson__answers">
        <div
            v-for="(answer, index) in currentQuestion.options"
            :key="answer"
            class="lesson__answer"
            :class="{
            correct: showResult && index === currentQuestion.correctIndex,
            wrong:
              showResult &&
              selectedIndex === index &&
              index !== currentQuestion.correctIndex
          }"
            @click="selectAnswer(index)"
        >
          {{ answer }}
        </div>
      </div>

      <button
          class="lesson__next-question"
          :disabled="!showResult"
          @click="nextQuestion"
      >
        Следующий вопрос
      </button>

      <button
          class="lesson__restart"
          @click="restartQuiz"
      >
        Начать заново
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref, computed } from 'vue'
  import { Questions } from '@/mocks/question'

  const currentQuestionIndex = ref(0)
  const userScore = ref(0)
  const selectedIndex = ref<number | null>(null)
  const showResult = ref(false)

  const currentQuestion = computed(() => {
    return Questions[currentQuestionIndex.value]
  })

  function selectAnswer(index: number) {
    if (showResult.value) return

    selectedIndex.value = index
    showResult.value = true

    if (index === currentQuestion.value.correctIndex) {
      userScore.value += 1
    }
  }

  function nextQuestion() {
    if (currentQuestionIndex.value < Questions.length - 1) {
      currentQuestionIndex.value += 1
      selectedIndex.value = null
      showResult.value = false
    }
  }

  function restartQuiz() {
    currentQuestionIndex.value = 0
    userScore.value = 0
    selectedIndex.value = null
    showResult.value = false
  }
</script>

<style scoped lang="scss">
  .lesson {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;

    &__wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    &__question {
      font-size: 32px;
      text-align: center;
    }

    &__score {
      margin-top: 24px;
      font-size: 22px;
      font-weight: 500;
      color: #1f2937;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    &__round {
      margin-top: 6px;
      font-size: 18px;
      font-weight: 400;
      color: #6b7280;
    }

    &__answers {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 16px 24px;
      margin-top: 40px;
    }

    .lesson__score::before {
      content: '🏆 ';
    }

    .lesson__round::before {
      content: '❓ ';
    }

    &__answer {
      min-width: 220px;
      padding: 10px 18px;
      background-color: #fff;
      color: #000;
      font-size: 20px;
      border-radius: 12px;
      box-shadow: rgba(0, 0, 0, 0.25) 0px 5px 15px;
      cursor: pointer;
      transition: all 0.25s ease;

      &:hover {
        transform: scale(1.05);
      }

      &.correct {
        background-color: #2ecc71;
        color: #fff;
      }

      &.wrong {
        background-color: #e74c3c;
        color: #fff;
      }
    }

    &__next-question {
      margin-top: 24px;
      padding: 0 28px;
      height: 44px;
      background-color: #405cf5;
      border-radius: 8px;
      border: none;
      color: #fff;
      font-size: 16px;
      cursor: pointer;

      &:disabled {
        background-color: #999;
        cursor: not-allowed;
      }
    }

    &__restart {
      margin-top: 12px;
      padding: 0 28px;
      height: 40px;
      background-color: #e67e22;
      border-radius: 8px;
      border: none;
      color: #fff;
      font-size: 15px;
      cursor: pointer;
      transition: 0.2s;

      &:hover {
        background-color: #d35400;
      }
    }
  }
</style>
