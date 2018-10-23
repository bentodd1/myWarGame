<template>
  <q-page class="flex flex-left">
    <q-card>
      <img alt="Quasar logo" src="~assets/quasar-logo-full.svg">
       <q-btn
    @click="startGame" >
      Start game
      </q-btn>
       <div>Cards in deck {{this.player1.deck.length}} Cards in discard {{this.player1.discardDeck.length}}</div>
       <div>{{player1.currentCard}}</div>
    </q-card>
      <q-card>
      <img alt="Quasar logo" src="~assets/quasar-logo-full.svg">
          <q-btn
      @click="flipCards([])">
       Flip flipCards
      </q-btn>
      <div>Cards in deck {{this.player2.deck.length}} Cards in discard {{this.player2.discardDeck.length}}</div>
      <div>{{player2.currentCard}}</div>
    </q-card>
  </q-page>
</template>

<style>
</style>

<script>
import starterDeck from '../assets/deck'
export default {
  name: 'PageIndex',
  data () {
    return {
      player1: {
        deck: [],
        discardDeck: [],
        currentCard: 0
      },
      player2: {
        deck: [],
        discardDeck: [],
        currentCard: 0
      },
      mainDeck: []
    }
  },
  methods: {

    async startGame () {
      this.mainDeck = starterDeck
      await this.shuffle(this.mainDeck)
      console.log('Game has started')
      console.log(this.mainDeck)
      await this.dealToEachPlayer()
    },

    dealToEachPlayer () {
      let splitDeck = this.mainDeck.splice(26)
      this.player1.deck = splitDeck
      this.player2.deck = this.mainDeck
      console.log(splitDeck)
      console.log(this.mainDeck)
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
      player.discardDeck.push.apply(player.discardDeck, discardPile)
    },
    checkDeckSize (player) {
      if (player.deck.length > 0) {
        console.log(player.discardDeck.length)
      } else if (player.deck.length === 0) {
        if (player.discardDeck.length < 1) {
          console.log('game is over you loose')
        } else {
          console.log('Shuffling deck into deck')
          player.deck.push.apply(player.deck, player.discardDeck)

          player.discardDeck = []
        }
      }
    },
    async flipCards (discardPile) {
      await this.checkDeckSize(this.player1)
      await this.checkDeckSize(this.player2)

      console.log(this.player1.discardDeck)
      console.log(this.player2.discardDeck)
      let player1Card = await this.player1.deck.shift()
      this.player1.currentCard = player1Card
      let player2Card = await this.player2.deck.shift()
      this.player2.currentCard = player2Card
      let player1CardValue = player1Card.charAt(0)
      let player2CardValue = player2Card.charAt(0)
      discardPile.push(this.player1Card)
      discardPile.push(this.player2Card)
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
