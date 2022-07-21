<script setup>
import {ref, computed} from 'vue'

const questions = ref([

  {
    question: 'Bilgenin en sevdiği kitap hangisi?',
    answer:2,
    Options:[
      'Tutunamayanlar',
      'Genç weatherin acıları',
      'Şeker portakalı'
    ],
    selected:null
  },
  {
    question: 'Bilgenin idolü kimdir?',
    answer:1,
    Options:[
      'Steve Jobs',
      'Bill Gates',
      'Elon Musk'
    ],
    selected:null
  },
   {
    question: 'Bilgenin en sevdiği içecek hangisi?',
    answer:0,
    Options:[
      'Kahve',
      'Meyve Suyu',
      'Ayran'
    ],
    selected:null
  },
   {
    question: 'Bilgenin en sevdiği çiçek hangisi?',
    answer:1,
    Options:[
      'Gül',
      'Papatya',
      'Çiçek sevmiyor'
    ],
    selected:null
  },
   {
    question: 'Bilgenin en sevdiği araba markası hangisi?',
    answer:1,
    Options:[
      'Mercedes',
      'Tesla',
      'BMW'
    ],
    selected:null
  },
   {
    question: 'Bilgenin en büyük hayali nedir?',
    answer:2,
    Options:[
      'japonyaya gitmek',
      'ölmek',
      'iyi bir yazılımcı olmak'
    ],
    selected:null
  },
   {
    question: 'Bilgenin en korktuğu hayvan hangisidir?',
    answer:0,
    Options:[
      'hamam böceği',
      'yılan',
      'akrep'
    ],
    selected:null
  },
  {
    question: 'Bilgenin en sevdiği aktör hangisidir?',
    answer:1,
    Options:[
      'Nicola kidman',
      'jim carrey',
      'Matthew McConaughey'
    ],
    selected:null
  },
  {
    question: 'Bilgenin en sevdiği yemek hangisidir?',
    answer:2,
    Options:[
      'sarma',
      'tantuni',
      'mantı'
    ],
    selected:null
  },
  {
    question: 'Bilgenin en büyük hayali hangisidir?',
    answer:0,
    Options:[
      'NASA da çalışmak',
      'Google da çalışmak',
      'Üniversitede çalışmak'
    ],
    selected:null
  }
  
  
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected != null && q.answer == q.selected) {
      console.log('correct');
      value++
    
    }

  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question= questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value
  e.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
    return
    
  }
    quizCompleted.value = true
  
}
</script>

<template>
 <main class="app">

  <h1>Bilgeyi Ne Kadar İyi Tanıyorsun</h1>

  <section class="quiz" v-if="!quizCompleted">
    <div class="quiz-info">
      <span class="question">{{getCurrentQuestion.question}}</span>
      <span class="score"> Score {{score}}/{{questions.length}}</span>
    </div>

    <div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.Options"
          :key="(option, index).id"
					:for="'option' + index" 
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
        :id="'option' + index"
        :name="getCurrentQuestion.index"
        :value="index"
        v-model="getCurrentQuestion.selected"
        :disabled="getCurrentQuestion.selected"
        @change="SetAnswer"
        />
        <span>{{ option }}</span>
      </label>

    </div>

    <button
      @click="NextQuestion"
      :disabled = "!getCurrentQuestion.selected">
      {{
        getCurrentQuestion.index == questions.length - 1
            ? 'bitti'
            : getCurrentQuestion.selected == null
                ? 'Seçeneği işaretleyin'
                : 'diğer soru'
      }}
    </button>

  </section>

  <section v-else>
    <h2>Tebrikler quizi bitirdiniz!</h2>
    <p>Skorunuz {{score}}/{{questions.length}}</p>
  </section>

 </main>
</template>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;

}

body {
  background-color: sienna;
  color: black;
}
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 5rem;
  height: 100vh;
}
h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  background-color: darkolivegreen;
  padding: 3rem;
  width: 150%;
  max-width: 640px;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info.question {
  color: black;
  font-size: 1.25rem;
}

.quiz-info.score {
  color: darkred;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 2rem;

}

.option {
  padding: 1rem;
  display: block;
  background-color: darkkhaki;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option-hover {
  background-color: azure;
}

.option.correct {
  background-color: beige;
}

.option.wrong {
  background-color: rgb(240, 9, 9);
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
  border: 5px;
  cursor: pointer;
  padding: 1 rem 2rem;
  background-color: rgb(141, 236, 86);
  color: rgb(117, 15, 15);
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
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
  color: rgb(173, 20, 0);
  font-size: 1.5rem;
  text-align: center;
}
</style>
