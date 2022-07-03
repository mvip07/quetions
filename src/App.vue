<script setup>
import { computed, ref } from "vue";

const questions = ref([
  {
    question: "What is Vue.js?",
    answer: 1,
    options: [
      "Javascript library",
      "Javascript framework",
      "Frontend programming language",
    ],
    selected: null,
  },

  {
    question: "What are the advantages of using Vue.js ?",
    answer: 2,
    options: [
      "Easily understandable — One of the reasons for the popularity of this framework is that it is quite easy to understand. Users can easily add Vue.js to their web project due to its simple structure.",
      "Straightforward integration- you can easily integrate it with the existing applications.",
      "All of the above",
    ],
    selected: null,
  },

  {
    question:
      "If you use .passive and .prevent together, .prevent will be ignored. Probably, it will show you a warning.",
    answer: 0,
    options: ["true", "false", "not clear"],
    selected: null,
  },

  {
    question: "Which of the directives below provide a two-way binding ?",
    answer: 2,
    options: ["none", "v-inline", "v-mode"],
    selected: null,
  },

  {
    question:
      "Which of the following directives is to used for attaching event listeners that invoke methods ?",
    answer: 1,
    options: ["v-model", "v-on", "v-bind"],
    selected: null,
  },

  {
    question:
      "What data binding interpolation is commonly known as “Mustache” syntax ?",
    answer: 1,
    options: ["[]", "{{}}", "v-on"],
    selected: null,
  },

  {
    question: "How to use for loop in Vue.js ?",
    answer: 1,
    options: ["*v-for", "v-for", "vFor"],
    selected: null,
  },

  {
    question: "What is the difference between v-html and v-text ?",
    answer: 2,
    options: [
      "v-text sets the textContent of the node",
      "v-html sets the innerHTML of the element",
      "Both of the statements above are true.",
    ],
    selected: null,
  },

  {
    question: "What is the correct way of installing the vue cli globally ?",
    answer: 1,
    options: [
      "npm install vue-cli",
      "npm install -g vue-cli",
      "npm install vue-cli -global",
    ],
    selected: null,
  },

  {
    question:
      "Which of the following event modifiers is applied only for prevent clicks on the element itself?",
    answer: 0,
    options: ["@click.prevent", "@click.stop", "@click.self.prevents"],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.answer == q.selected) {
      console.log("correct");
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const setAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};
const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1)
    currentQuestion.value++;
  else quizCompleted.value = true;
};

const Restart = () => {
  window.location.reload();
};
</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }} / {{ questions.length }}</span>
      </div>
      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :for="'option' + index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'corrent'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="setAnswer"
          />
          <sapn>{{ option }}</sapn>
        </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Finish"
            : "Next Question"
        }}
      </button>
    </section>

    <section v-else>
      <h2>You have finished the quiz</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
      <button @click="Restart">Restart</button>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat";
}

.app {
  padding: 30px 50px;
  width: 100%;
  height: 100vh;
  background: #ececec;
}

h1 {
  margin-top: 30px;
  text-align: center;
}

section {
  padding: 30px 15px;
  margin: 20px auto;
  width: 70%;
  background: #f9f9f9;
  box-shadow: 3px 3px rgb(191, 191, 191), -1em 0 0.4em rgb(202, 202, 202);
}

.question {
  font-size: 20px;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  font-weight: 700;
}

.score {
  text-align: end;
  margin-top: 15px;
  display: block;
  font-size: 17px;
  font-weight: 600;
  color: #999;
}

.options label {
  display: block;
  padding: 12px 24px;
  margin: 10px 0;
  font-size: 17px;
  font-weight: 600;
}

.options label.corrent {
  background: rgb(0, 224, 0);
  color: #fff;
}

.options label.wrong {
  background: rgb(255, 0, 0);
  color: #fff;
}

.options label input {
  margin-right: 7px;
}

button {
  display: block;
  margin: 20px auto;
  width: 100%;
  padding: 8px 32px;
  border: none;
  color: #fff;
  background: black;
  border-radius: 7px;
  font-size: 18px;
  font-weight: 600;
}

button:disabled {
  background: rgb(0, 69, 0);
}
</style>
