<template>
  <div id="game">
    <h1>Game!</h1>
    <p v-once>Render this random number on refresh: {{ randomNumber() }}</p>
    <p>Render the number on button click: {{ showButtonNumber }}</p>
    <button v-on:click="getButtonNumber">Get a number!</button>
    <hr />
    <p>Get a new card from the array on button click: {{ cardValue }} {{ testValue }}</p>
    <button v-on:click="getCard">Get a card!</button>
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
      testValue: ""
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
    aValue() {
      return 'hello';
    },
    getCard() {
      // Check if array is empty here
      if (numbers.length > 0) {
        // Shuffle the parent array
        const randomCard = numbers.sort(() => Math.random() - 0.5);
        // Get the random number
        let number = randomCard[0][0];

        // Convert number to string if necessary
        if (number == "1") {
          number = 'Ace';
        } else if (number == '11') {
          number = 'Jack'
        } else if (number == '12') {
          number = 'Queen'
        } else if (number == '13') {
          number = 'King'
        }
        this.testValue = this.aValue();
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
