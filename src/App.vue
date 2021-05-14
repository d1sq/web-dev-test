<template>
  <div class="container-app">
    <div class="container-quiz">
      <div class="header-quiz">
        <h1>Лабораторная работа №4</h1>
      </div>
      <div class="step-progress" :style="{ width: progress + '%' }"></div>
      <div
        class="box"
        v-for="(question, index) in questions.slice(a, b)"
        :key="index"
        v-show="quiz"
      >
        <div class="box-question">
          <h2>Вопросы {{ b }}/{{ questions.length }}</h2>
          <p>{{ question.question }}</p>
        </div>
        <div class="box-propositions">
          <ul>
            <li
              v-for="(proposition, index) in question.propositions"
              :key="index"
              class="li neon"
              @click="selectResponse(proposition, index)"
              :class="correct ? check(proposition) : ''"
            >
              {{ proposition.props }}
              <div
                class="fas fa-check"
                v-if="correct ? proposition.correct : ''"
              ></div>
              <div
                class="fas fa-times"
                v-if="correct ? !proposition.correct : ''"
              ></div>
            </li>
          </ul>
        </div>
      </div>
      <div class="box-score" v-if="score_show">
        <h2>Your score is</h2>
        <h2>{{ score }}/{{ questions.length }}</h2>
        <h2>Ваша оценка - {{ mark }}</h2>
        <div class="btn-restart">
          <button @click="restartQuiz">
            Restart <i class="fas fa-sync-alt"></i>
          </button>
        </div>
      </div>
      <div class="footer-quiz">
        <div v-if="progress < 100" class="box-button">
          <button @click="skipQuestion(), getMark()">Skip</button>
          <button @click="nextQuestion(), getMark()">Next</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [
        {
          question: "Каким тегом обозначается написание сценария?",
          propositions: [
            { props: "<script>", correct: true },
            { props: "<js>" },
            { props: "<scripting>" },
            { props: "<idfk>" },
          ],
        },
        {
          question: "По какому стандарту пишется код в JS Web-разработке",
          propositions: [
            { props: "Записи в тетради с лекций web-разработки" },
            { props: "Php world standart" },
            { props: "ES6", correct: true },
            { props: "basic scripting rules" },
          ],
        },
        {
          question: "Как вывести Hello world диалог перед пользователем",
          propositions: [
            { props: 'msg("Hello World")' },
            { props: 'alertBox("Hello World")' },
            { props: 'alert("Hello World")', correct: true },
            { props: 'print("Hello World")' },
          ],
        },
        {
          question: "Расшифруйте аббривеатуру SASS",
          propositions: [
            { props: "Syntactically Awesome Stylesheets", correct: true },
            { props: "Super Adapted Style Sheets" },
            { props: "Some random fuk idk" },
            { props: "Cascading Style Shits boosted" },
          ],
        },
        {
          question: "Какой тег отвечает за заголовок таблицы",
          propositions: [
            { props: "<td></td>" },
            { props: "<table></table>" },
            { props: "<div></div>" },
            { props: "<th></th>", correct: true },
          ],
        },
      ],
      mark: 0,
      a: 0,
      b: 1,
      next: true,
      score_show: false,
      quiz: true,
      score: 0,
      correct: false,
      progress: 0,
    };
  },
  name: "App",

  methods: {
    selectResponse(e) {
      this.correct = true;
      this.next = false;
      if (e.correct) {
        this.score++;
      }
    },
    check(status) {
      if (status.correct) {
        return "correct";
      } else {
        return "incorrect";
      }
    },
    nextQuestion() {
      if (this.next) {
        return;
      }
      this.progress = this.progress + 100 / this.questions.length;
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
        this.correct = false;
        this.next = true;
      }
    },
    skipQuestion() {
      if (!this.next) {
        return;
      }
      this.progress = this.progress + 100 / this.questions.length;

      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
      }
    },

    restartQuiz() {
      Object.assign(this.$data, this.$options.data()); // reset data in vue
    },
    getMark() {
      if (this.score <= 2) {
        this.mark = "неудовлетворительно";
      }
      if (this.score == 3) {
        this.mark = "удовлетворительно";
      }
      if (this.score == 4) {
        this.mark = "Хорошо";
      }
      if (this.score == 5) {
        this.mark = "Отлично";
      }
    },
  },
};
</script>

<style src="./style.css"></style>
