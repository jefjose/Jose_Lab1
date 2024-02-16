<template>
  <div id="app">
    <h1>Rock Paper Scissors</h1>
    <div>
      <img src="/rock.jpg" alt="Rock" class="choice-img" @click="play('rock')">
      <img src="/paper.jpg" alt="Paper" class="choice-img" @click="play('paper')">
      <img src="/scissors.jpg" alt="Scissors" class="choice-img" @click="play('scissors')">
    </div>
    <div>
      <h3>VS</h3>
    </div>
    <div>
      <img :src="chosenImage" alt="Computer Choice" class="choice-img">
    </div>
    <div class="result">
      <p v-if="result !== null">{{ result }}</p>
      <p>Wins: {{ wins }}</p>
      <p>Ties: {{ ties }}</p>
      <p>Losses: {{ losses }}</p>
      <button @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      game: new Game(),
      chosenImage: '/placeholder.jpg'
    };
  },
  methods: {
    play(choice) {
      this.game.play(choice);
      if (this.game.computerChoice !== null) {
        this.chosenImage = '/' + this.game.computerChoice + '.jpg';
      }
    },
    reset() {
      this.game.reset();
      this.chosenImage = '/placeholder.jpg';
    }
  },
  
  computed: {
    result() {
      return this.game.result;
    },
    wins() {
      return this.game.wins;
    },
    ties() {
      return this.game.ties;
    },
    losses() {
      return this.game.losses;
    }
  }
};

class Game {
  constructor() {
    this.choices = ['rock', 'paper', 'scissors'];
    this.result = "Choose your pick!";
    this.computerChoice = null;
    this.wins = 0;
    this.ties = 0;
    this.losses = 0;
  }

  play(userChoice) {
    const computerChoice = this.choices[Math.floor(Math.random() * this.choices.length)];
    this.computerChoice = computerChoice;

    if (userChoice === computerChoice) {
      this.result = "It's a tie!";
      this.ties++;
    } else if (
      (userChoice === 'rock' && computerChoice === 'scissors') ||
      (userChoice === 'paper' && computerChoice === 'rock') ||
      (userChoice === 'scissors' && computerChoice === 'paper')
    ) {
      this.result = 'You win!';
      this.wins++;
    } else {
      this.result = 'Computer wins!';
      this.losses++;
    }
  }

  reset() {
    this.result = "Choose your pick!";
    this.computerChoice = null;
    this.wins = 0;
    this.ties = 0;
    this.losses = 0;
  }
}
</script>

<style scoped>
body {
  margin: 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

#app {
  text-align: center;
  position: fixed;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.choice-img {
  width: 100px;
  cursor: pointer;
  margin: 5px;
}

.result {
  position: fixed;
  top: 450px;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
