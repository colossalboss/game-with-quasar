<template>
  <q-page class="flex flex-center">
    <section style="margin-top: 20px;" v-if="gameStarted">
      <ul class="deck">
        <li class="card" @click="onCardClick($event)" v-for="icon in icons"><i :class="icon"></i></li>
      </ul>
    </section>

    <section v-if="!gameStarted">
      <div id="over" v-if="won">
        <p>Congrats! You won</p>
        <p>
          Moves {{ moves }} <br>
          Time {{ duration }} seconds <br>
        <ul class="rate ratings" >
          <li v-for="i in stars"><i class="fa fa-star"></i></li>
        </ul>
        </p>
        <button id="again" @click="playAgain">{{ games > 0 ? 'Restart' : 'Play'}}</button>
      </div>
    </section>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      openCards: [],
      matchedCards: [],
      duration: 0,
      timerInterval: 0,
      moves: 0,
      games: 0,
      stars: 5,

      gameStarted: false,
      won: true,
      icons: [
        "fa fa-cube",
        "fa fa-leaf",
        "fa fa-leaf",
        "fa fa-bicycle",
        "fa fa-bicycle",
        "fa fa-bomb",
        "fa fa-diamond",
        "fa fa-diamond",
        "fa fa-paper-plane-o",
        "fa fa-paper-plane-o",
        "fa fa-anchor",
        "fa fa-anchor",
        "fa fa-bolt",
        "fa fa-bolt",
        "fa fa-cube",
        "fa fa-bomb",
      ]
    }
  },

  methods: {
    onCardClick(event) {
      const currentCard = event.srcElement;
      const previousCard = this.openCards[0];

      if (this.openCards.length === 1) {
        event.srcElement.classList.add("open", "show", "disable");
        this.openCards.push(event.srcElement);
        this.compareCards(currentCard, previousCard);
      } else {
        event.srcElement.classList.add("open", "show", "disable");
        this.openCards.push(event.srcElement);;
      }
    },

    flushOpenCards() {
      this.openCards = [];
    },

    compareCards(currentCard, previousCard) {
      console.log(currentCard.innerHTML === previousCard.innerHTML, "match?");
      if (currentCard.innerHTML === previousCard.innerHTML) {
        currentCard.classList.add('match');
        previousCard.classList.add('match');
        this.matchedCards.push(currentCard, previousCard);
        this.openCards = [];
        this.gameOver();
      } else {
        currentCard.classList.add('red');
        previousCard.classList.add('red');
        setTimeout(function () {

          currentCard.classList.remove('open', 'show', 'disable');
          previousCard.classList.remove('open', 'show', 'disable');

          currentCard.classList.remove('red');
          previousCard.classList.remove('red');

          // this.flushOpenCards();
        }, 500);
        this.flushOpenCards();
        this.moves = this.moves += 1;
      }
    },

    changeValue() {
      this.duration += 1;
    },

    start() {
      this.duration = 0;
      this.timerInterval = setInterval(this.changeValue, 1000);
    },

    stop() {
      clearInterval(this.timerInterval);
      this.timerInterval = null;
      this.gameStarted = false;
    },

    gameOver() {
      if (this.matchedCards.length === this.icons.length) {
        this.games += 1;
        setTimeout(() => {
          this.gameStarted = false;
        }, 500);

        stop();
      }
    },

    playAgain() {
      this.gameStarted = false;
      // stop timer
      this.stop();
      // create new cards
      this.gameStarted = true;
      this.start();

      // reset other variables
      this.duration = 0;
      this.matchedCards = [];
      this.moves = 0;
    }
  }
}
</script>

<style scoped>
html {
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  background: #ffffff url('../assets/geometry2.png');
  /* Background pattern from Subtle Patterns */
  font-family: 'Coda', cursive;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

h1 {
  font-family: 'Open Sans', sans-serif;
  font-weight: 300;
}

/*
 * Styles for the deck of cards
 */

.deck {
  width: 660px;
  min-height: 680px;
  background: linear-gradient(160deg, #02ccba 0%, #aa7ecd 100%);
  padding: 32px;
  border-radius: 10px;
  box-shadow: 12px 15px 20px 0 rgba(46, 61, 73, 0.5);
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  margin: 0 0 3em;
}

.deck .card {
  height: 125px;
  width: 125px;
  background: #2e3d49;
  font-size: 0;
  color: #ffffff;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 5px 2px 20px 0 rgba(46, 61, 73, 0.5);
}

.deck .card.open {
  transform: rotateY(0);
  background: #02b3e4;
  cursor: default;
}

.deck .card.show {
  font-size: 33px;
}

.deck .card.match {
  cursor: default;
  background: #02ccba;
  font-size: 33px;
}

.deck .card.disable {
  pointer-events: none;
}

/*
 * Styles for the Score Panel
 */

.score-panel {
  text-align: left;
  width: 345px;
  margin-bottom: 10px;
}

.score-panel .stars {
  margin: 0;
  padding: 0;
  display: inline-block;
  margin: 0 5px 0 0;
}

.score-panel .stars li {
  list-style: none;
  display: inline-block;
}

.score-panel .restart {
  float: right;
  cursor: pointer;
}

#over {
  width: 100%;
  text-align: center;
  margin: auto;
}

#again {
  background-color: green;
  color: #fff;
  padding: 8px;
}

#again:hover {
  cursor: pointer;
}

.rate li {
  list-style: none;
  display: inline;

}

.red {
  background-color: red !important;
}

.ratings {
  list-style: none;
  display: flex;
  padding: 0;
  justify-content: center;
}
</style>
