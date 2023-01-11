<template>
  <div class="hello">
    <div class="about">
      <h3>Угадайте слово из 5 букв.</h3>
      <p>
        Угадайте слово из 5 букв за 5 попыток!<br>
        Если буква подсвечивается серым цветом - она есть в загаданном слове, но стоит на другом месте.<br>
        Если буква подсвечивается зеленым цветом - она есть в загаданном слове и стоит на правильном месте. </p>
    </div>
    <h4>Осталось попыток: {{ lives }}</h4>
    <div class="input-block">
      <input
          v-for="(item) in userWord"
          :key="item.id"
          type="text"
          v-model="item.letter"
          v-on:keypress="isLetter($event)"
          maxlength="1"
          :disabled="item.disabled"
          :class="{ entry: item.entry }"
      >
    </div>
    <button class="btn" @click="checkWord()" v-if="!gameover">Проверить</button>
    <button class="btn" @click="restart()" v-else>Еще раз</button>
    <div class="message">
      {{ message }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameBlock',
  data() {
    return {
      gameover: false,
      lives: 5,
      randomWord: 0,
      message: '',
      secretWord: ['полка', 'бекон', 'волна', 'гений', 'земля','забор','князь','обман','карта','салат','укроп','фокус'],
      userWord: [
        {
          id: 0,
          letter: '',
          entry: false,
          disabled: false,
        },
        {
          id: 1,
          letter: '',
          entry: false,
          disabled: false,
        },
        {
          id: 2,
          letter: '',
          entry: false,
          disabled: false,
        },
        {
          id: 3,
          letter: '',
          entry: false,
          disabled: false,
        },
        {
          id: 4,
          letter: '',
          entry: false,
          disabled: false,
        }
      ],
      finalWord: '',
    }
  },
  props: {

  },
  mounted() {
    //При первом запуске игры генерируем случайное число для выбора первого слова
    this.randomWord = Math.floor(Math.random() * (this.secretWord.length - 1) * 1);
  },
  methods: {
    checkWord() {
      //Уменьшаем количетсво попыток
      this.lives--;

      //Если жизни раны 0, то игра завершена
      if (this.lives === 0) {
        this.gameover = true;
      }

      /*
        Проверяем, есть ли среди введеных букв те, что входят в загаданое слово.
        Если да, они подсвечиваются серым
      */
      this.userWord.forEach((item) => {
        if (this.secretWord[this.randomWord].includes(item.letter) && item.letter !== '') {
          item.entry = true;
        }
      })

      /*
        Проверяем, есть ли среди введеных букв те, что входят
        в загаданое слово и стоят ли они на верной позиции.
        Если да, они подсвечиваются зеленым и блокируются.
      */
      this.userWord.map((item, index) => {
        if (item.letter === this.secretWord[this.randomWord][index]) {
            item.disabled = true;
        }
      })

      //Записываем введеное пользователем слово
      this.finalWord = this.userWord.map((item) => item.letter).join('');

      //Проверяем, совпадает ли введеное слово с загаданным. Если да, то останавливаем игру
      if (this.secretWord[this.randomWord] === this.finalWord.toLowerCase()) {
        this.message = 'Угадал';
        this.gameover = true;
      } else {
        this.message = 'Пробуй еще';
      }

    },
    isLetter(e) {
      //Ограничиваем ввод только кириллицы
      let char = String.fromCharCode(e.keyCode);
      if(/^[а-яА-ЯёЁ]+$/.test(char)) return true;
      else e.preventDefault();
    },
    restart() {
      //Перезапуск игры. Переменные возвращаются в исходное положение
      this.lives = 5;
      this.message = '';
      this.gameover = false;
      this.userWord.forEach((item) => {
        item.letter = '';
        item.entry = false;
        item.disabled = false;
      })
      this.newRandom();
    },
    newRandom() {
      //Генерируем новое случайное число
      this.randomWord = Math.floor(Math.random() * (this.secretWord.length - 1) * 1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.input-block {

}

.input-block input {
  margin: 10px;
  width: 50px;
  height: 50px;
  font-size: 50px;
  text-align: center;
  border-radius: 10px;
  border: 2px solid #ff8100;
}

.input-block input.entry {
  background: #d3d3d3;
}

.input-block input:disabled {
  background: #bcffb9;
}

.btn {
  background: #fff;
  border: 2px solid #41b883;
  border-radius: 10px;
  padding: 10px 30px;
  font-size: 16px;
  cursor: pointer;
}

.btn:hover {
  border: 2px solid #2c3e50;
}
</style>
