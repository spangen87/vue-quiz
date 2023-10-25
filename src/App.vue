<script setup>
import { ref, computed} from 'vue'

const questions = ref([
  {
    question: 'När grundades Best Lås?',
    answer: 2,
    options: [
      '1891',
      '1949',
      '1974',
      '1983',
    ],
    selected: null
  },
  {
    question: 'Vilka jobbar Best Lås INTE med?',
    answer: 3,
    options: [
      'CES',
      'BEST',
      'EVVA',
      'ASSA',
      'ILOQ',
    ],
    selected: null
  },
  {
    question: 'Vilka är ledorden?',
    answer: 0,
    options: [
      'Pålitlighet, driftighet och kvalité',
      'Pålitlighet, driftighet och anpassning',
      'Pålitlighet, driftighet och flexibilitet',
    ],
    selected: null
  },
  {
    question: 'Vem grundade Best Sverige?',
    answer: 3,
    options: [
      'Fredrik Hägglund',
      'Lennart Sjökvist',
      'Hans Edling',
      'Hans Jarkell',
    ],
    selected: null
  },
  {
    question: 'När öppnade kontoret i Falun?',
    answer: 2,
    options: [
      '2006',
      '2009',
      '2011',
      '2013',
    ],
    selected: null
  },
  {
    question: 'Vad heter teknikerna i Stockholm?',
    answer: 3,
    options: [
      'Knatte, Fnatte och Tjatte',
      'Dom tre musketörerna',
      'Bröderna dalton',
      'Kenneth, Fredrik och Robin',
    ],
    selected: null
  },
  {
    question: 'Best Lås & Säkerhetssystem AB nuvarande VD?',
    answer: 0,
    options: [
      'Daniel Edling',
      'Järker Edling',
      'Hans Edling',
      'Fredrik Reinfeldt',
    ],
    selected: null
  },
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted.value = true
  }
}

</script>

<template>
  <main class="app">
    <h1>The Best Quiz</h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Poäng {{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label 
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`">

          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer">
          <span>{{ option }}</span>
        </label>
      </div>

      <button 
        @click="NextQuestion"
        :disabled="!getCurrentQuestion.selected">
        {{ 
          getCurrentQuestion.index == questions.length - 1
            ? 'Slutför'
            : getCurrentQuestion.selected == null
              ? 'Välj ett alternativ'
              : 'Nästa fråga'
        }}
      </button>
    </section>

    <section v-else>
      <h2>Du har slutfört quizzet!</h2>
      <p>Din poäng blev {{ score }}/{{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #271c36;
  color: #fff;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quiz-info .score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;

}

.option {
  display: block;
  padding: 1rem;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #2d213f;
}

.option.correct {
  background-color: #2cce7d;
}

.option.wrong {
  background-color: #ff5a5f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;

  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button:disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align: center;
}

</style>
