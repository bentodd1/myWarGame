<template>
  <q-page class="flex flex-center">
    <q-card>
      <img alt="Quasar logo" src="~assets/quasar-logo-full.svg">
      <div>My Value</div>
      <div>Deck size</div>
    </q-card>
    <q-btn
    @click="startGame" >
      Start game
      </q-btn>
      <q-btn
      @click="flipCards([])">
       Flip flipCards
      </q-btn>
      <q-card>
      <img alt="Quasar logo" src="~assets/quasar-logo-full.svg">
      <div>deck size</div>
    </q-card>
  </q-page>
</template>

<style>
</style>

<script>
import deck from '../assets/deck'
export default {

  name: 'PageIndex',
  data () {
    return {
      player1: {
        deck: [],
        discardDeck: []
      },
      player2: {
        deck: [],
        discardDeck: []
      }
    }
  },
  methods: {
    async startGame () {
      await this.shuffle(deck)
      console.log('Game has started')
      console.log(deck)
      await this.dealToEachPlayer()
    },

    dealToEachPlayer () {
      let splitDeck = deck.splice(26)
      this.player1.deck = splitDeck
      this.player2.deck = deck
      console.log(splitDeck)
      console.log(deck)
    },
    shuffle (array) {
      for (var i = array.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1))
        var temp = array[i]
        array[i] = array[j]
        array[j] = temp
      }
    },
    fighWar (discardPile) {
      for (let i = 0; i < 3; i++) {
        discardPile.push(this.player1.deck.shift())
        discardPile.push(this.player2.deck.shift())
      }
      this.flipCards(discardPile)
    },
    takeDiscardPile (player, discardPile) {
      player.discardDeck.concat(discardPile)
    },
    checkDeckSize (player) {
      if (player.deck.length > 0) {
      } else if (player.discardDeck > 0 && player.deck.length === 0) {
        player.deck.concat(player.discardDeck)
        player.discardDeck = []
      } else {
        console.log('game is over you loose')
      }
    },
    async flipCards (discardPile) {
      await this.checkDeckSize(this.player1)
      await this.checkDeckSize(this.player2)

      console.log(this.player1.discardDeck)
      console.log(this.player2.discardDeck)
      let player1Card = await this.player1.deck.shift()
      let player2Card = await this.player2.deck.shift()
      let player1CardValue = player1Card.charAt(0)
      let player2CardValue = player2Card.charAt(0)
      discardPile.push(player1Card)
      discardPile.push(player2Card)
      if (player1CardValue > player2CardValue) {
        console.log('player 1 card is larger')
        this.takeDiscardPile(this.player1, discardPile)
      } else if (player1CardValue < player2CardValue) {
        this.takeDiscardPile(this.player2, discardPile)
        console.log('player2CardIs larger')
      } else if (player1CardValue === player2CardValue) {
        this.fighWar(discardPile)
        console.log('War')
      }
    }
  }
}
</script>
