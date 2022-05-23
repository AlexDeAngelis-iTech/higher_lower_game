<template>
  <div id="game">
    <h1>Game!</h1>
    <p v-once>Render this random number on refresh: {{ randomNumber() }}</p>
    <p>Render the number on button click: {{ showButtonNumber }}</p>
    <button v-on:click="getButtonNumber">Get a number!</button>
    <hr />
    <p>Get a new card from the array on button click: {{ cardValue }}</p>
    <button v-on:click="getCard">Get a card!</button>
    <p>Result: {{ result }}</p>
  </div>
</template>

<script>
import numbers from "../cards";

export default {
  data() {
    return {
      text: "Hello!",
      showButtonNumber: "",
      cardValue: "",
      currentValue: "",
      previousValue: "",
      result: "",
    };
  },
  methods: {
    randomNumber() {
      const randomNumber = Math.ceil(Math.random() * 100);
      console.log(randomNumber);
      return randomNumber;
    },
    getButtonNumber() {
      this.showButtonNumber = Math.ceil(Math.random() * 100);
    },
    evaluateCardValues() {
      if (this.currentValue && this.previousValue) {
        if (this.currentValue > this.previousValue) {
          // Higher
          return 'Higher! As ' + this.currentValue + ' is more than ' + this.previousValue + '!';
        } else if (this.currentValue < this.previousValue) {
          // Lower
          return 'Lower! As ' + this.currentValue + ' is less than ' + this.previousValue + '!';
        } else {
          // Draw
          return 'Draw! As ' + this.currentValue + ' is the same as ' + this.previousValue + '!';
        }
      } else {
        return 'not enough data!';
      }
    },
    getCard() {

      // If a currentValue exists, now set it as the previousValue
      if (this.currentValue) {
        this.previousValue = this.currentValue;
      }

      // Check if array is empty here
      if (numbers.length > 0) {
        // Shuffle the parent array
        const randomCard = numbers.sort(() => Math.random() - 0.5);
        // Get the random number
        let number = randomCard[0][0];
        // Set currentValue as the number
        this.currentValue = number;
        // Convert number to string if necessary
        if (number === 1) {
          number = 'Ace';
        } else if (number === 11) {
          number = 'Jack'
        } else if (number === 12) {
          number = 'Queen'
        } else if (number === 13) {
          number = 'King'
        }
        this.result = this.evaluateCardValues();
        const suits = randomCard[0][1];

        // Check if the suits array is empty here
        if (suits.length > 0) {
          const randomSuit = suits.sort(() => Math.random() - 0.5);
          const suit = randomSuit[0];
          this.cardValue = number + ' of  ' + suit;
          suits.splice(0, 1);
          // Check if the suit array is empty here
          if (suits.length == 0) {
            // Remove the card number from the game
            numbers.splice(0, 1);
          }
        }
      } else {
        console.log("No more cards");
      }
    }
  },
};
</script>
