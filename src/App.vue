<template>
  <div>
    <input
      :placeholder="currentPlaceholder"
      v-model="userInput"
      @keydown="handleKeydown"
      :style="inputStyle"
      ref="input"
      @blur="handleBlur"
      @focus="handleFocus"
      @mousemove="avoidMouse"
    />
    <p v-if="isCorrect">🎉 Congratulations! You've typed the correct sentence. 🎉</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userInput: '',
      correctSentence: 'this is a relly long text to type',
      isCorrect: false,
      currentPlaceholder: 'Start typing...',
      inputStyle: {
        position: 'relative',
        transition: 'all 0.3s',
        fontSize: '1.2em',
      },
      isInputHidden: false,
      distractions: ['Try harder!', 'You almost got it!', 'Keep going!', 'Focus...'],
    };
  },
  created() {
    setInterval(() => {
      if (Math.floor(Math.random() * 2)) {
        document.body.classList.toggle("hideCursor");
      }
    }, 5000);
  },
  methods: {
    handleKeydown() {
      this.randomlyMoveInput();
      this.changePlaceholder();
      this.swapCharacters();
      this.randomlyInsertCharacter();
      this.invertTyping();
      this.dynamicFontSize();
      this.showDistractions();
      this.checkCorrectness();
    },
    handleBlur() {
      setTimeout(() => {
        this.$refs.input.focus();
      }, 500);
    },
    handleFocus() {
      this.currentPlaceholder = 'Keep going...';
    },
    randomlyMoveInput() {
      this.inputStyle.left = `${Math.random() * 30}dvw`;
      this.inputStyle.top = `${Math.random() * 80}dvh`;
    },
    changePlaceholder() {
      const placeholders = [
        'Almost there...',
        'Keep trying...',
        'You can do it!',
        'Not quite right...',
      ];
      this.currentPlaceholder = placeholders[Math.floor(Math.random() * placeholders.length)];
    },
    swapCharacters() {
      if (this.userInput.length > 1 && Math.random() < 0.2) {
        const lastIndex = this.userInput.length - 1;
        const swappedInput =
          this.userInput.slice(0, lastIndex - 1) +
          this.userInput[lastIndex] +
          this.userInput[lastIndex - 1];
        this.userInput = swappedInput;
      }
    },
    randomlyInsertCharacter() {
      if (Math.random() < 0.1) {
        const randomChar = String.fromCharCode(97 + Math.floor(Math.random() * 26));
        this.userInput += randomChar;
      }
    },
    invertTyping() {
      if (Math.random() < 0.05) {
        const invertedChar = String.fromCharCode(122 - (this.userInput.charCodeAt(this.userInput.length - 1) - 97));
        this.userInput = this.userInput.slice(0, -1) + invertedChar;
      }
    },
    dynamicFontSize() {
      const fontSize = Math.random() * 2 + 1;
      this.inputStyle.fontSize = `${fontSize}rem`;
    },
    showDistractions() {
      if (Math.random() < 0.1) {
        alert(this.distractions[Math.floor(Math.random() * this.distractions.length)]);
      }
    },
    avoidMouse() {
      if (Math.random() < 0.1) {
        this.inputStyle.left = `${Math.random() * 20}dvw`;
        this.inputStyle.top = `${Math.random() * 80}dvh`;
      }
    },
    checkCorrectness() {
      if (this.userInput === this.correctSentence) {
        this.isCorrect = true;
      }
    },
  },
};
</script>

<style lang="scss">
  body{
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
  }
  .hideCursor{
    cursor: none;
    input{
      cursor: none;
    }
  }
  input{
    display: block;
    width: 240px;
    height: 35px;
    outline: none;
    border: 1px solid silver;
    border-radius: 4px;
    padding-left: 15px;
    transition: all .3s ease-out;
    &:disabled{
      opacity: .4;
    }
  }
</style>