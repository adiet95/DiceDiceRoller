<template>
   <div class="game">
    <Banner></Banner>
    <div class="mx-2">
        <h2 class="player-info">{{active}}</h2>
      </div>
    <Player
      :players="players"
      :playerPoints="playerPoints"
    ></Player>
    <CurrentScore
      :buffer="data.pointBuffer"
    ></CurrentScore>
    <div class="row d-flex" v-if="active == this.$store.state.playerName">
      <div class="col">
        <h3 @click="roll">Roll</h3>
      </div>
      <div class="col">
        <h3 @click="accept">Hold</h3>
      </div>
    </div>
    <div class="container">
    <DiceContainer
      :firstDice="data.firstDice"
      :secondDice="data.secondDice"
    ></DiceContainer>
    <ChatBox></ChatBox>
    </div>
  </div>
</template>

<script>
import Banner from '../components/diceBanner'
import Player from '../components/playerCards'
import DiceContainer from '../components/diceContainer'
import CurrentScore from '../components/CurrentScore'
import ChatBox from '../components/chatBox'
export default {
  name: 'DiceGamble',
  components: {
    Banner,
    Player,
    DiceContainer,
    CurrentScore,
    ChatBox
  },
  data: function () {
    return {
      active: '',
      playerPoints: {},
      data: {
        firstDice: 0,
        secondDice: 0,
        pointBuffer: 0
      }
    }
  },
  methods: {
    roll () {
      this.$socket.emit('roll')
    },
    nextTurn () {
      this.$socket.emit('nextTurn')
    },
    accept () {
      this.$socket.emit('accept', { player: this.active, point: this.data.pointBuffer })
      this.data.pointBuffer = 0
    }
  },
  sockets: {
    roll (data) {
      this.data = data
    },
    setActive (name) {
      this.active = name
    },
    setPoint (points) {
      this.playerPoints = points
    },
    win (value) {
      this.$store.commit('isWin', value)
      this.$router.push('/win')
    }
  },
  watch: {
  },
  computed: {
    players () {
      return this.$store.state.players
    }
  }
}
</script>

<style>

</style>
