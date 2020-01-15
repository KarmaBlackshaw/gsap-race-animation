<template>
  <div class="layout">
    <div class="title-bar">
      <h1>TweenMax</h1>
      <h1>{{ timer }}s</h1>
    </div>
    <div class="race-ground">
      <div class="light-container">
        <div class="light light-1" />
        <div class="light light-2" />
        <div class="light light-3" />
      </div>

      <div class="lane">
        <div class="snail snail-1">{{ userBet === 1 ? '' : '' }}</div>
      </div>
      <div class="lane">
        <div class="snail snail-2">{{ userBet === 2 ? '' : '' }}</div>
      </div>
      <div class="lane">
        <div class="snail snail-3">{{ userBet === 3 ? '' : '' }}</div>
      </div>
    </div>

    <hr />
    <center>Select a first class car.</center>
    <div class="bet-container">
      <div
        class="bet bet-1"
        :class="{ active: userBet !== 1 && userBet !== null }"
        @click="placeBet(1)"
      >
        <img
          src="@/assets/car1.png"
          class="bet-img"
          alt=""
          :width="userBet === 1 ? '70px' : '50px'"
        />
      </div>
      <div
        class="bet bet-2"
        :class="{ active: userBet !== 2 && userBet !== null }"
        @click="placeBet(2)"
      >
        <img
          src="@/assets/car2.png"
          class="bet-img"
          alt=""
          :width="userBet === 2 ? '70px' : '50px'"
        />
      </div>
      <div
        class="bet bet-3"
        :class="{ active: userBet !== 3 && userBet !== null }"
        @click="placeBet(3)"
      >
        <img
          src="@/assets/car3.png"
          class="bet-img"
          alt=""
          :width="userBet === 3 ? '70px' : '50px'"
        />
      </div>
    </div>

    <button
      class="btn btn-start"
      v-text="'START'"
      @click="start"
      :disabled="isTweening"
    />
  </div>
</template>

<script>
import { TweenMax, gsap } from 'gsap'
import Swal from 'sweetalert2'

export default {
  name: 'home',

  data() {
    return {
      snail: null,
      lane: null,
      snails: null,
      lanes: null,
      snailWidth: null,
      laneWidth: null,
      finishLine: null,
      heirarchy: 0,
      tween: null,
      userBet: null,
      eases: [
        'circ.inOut',
        'expo.inOut',
        'sine.inOut',
        'power1.inOut',
        'power2.inOut',
        'power3.inOut',
        'power4.inOut',
        'slow(0.3, 0.4, false)'
      ],
      timer: 0,
      timerInterval: null
    }
  },

  mounted() {
    this.tween = gsap
    this.lanes = document.getElementsByClassName('lane')
    this.snails = document.getElementsByClassName('snail')
    this.lane = this.lanes[0]
    this.snail = this.snails[0]
    this.laneWidth = this.lane.offsetWidth
    this.snailWidth = this.snail.offsetWidth
    this.finishLine = this.laneWidth - this.snailWidth
  },

  computed: {
    isTweening() {
      return this.snails ? this.tween.isTweening(this.snails) : false
    }
  },

  methods: {
    reset() {
      this.heirarchy = 0
      this.tween.set(this.snails, { x: 0 })
      this.timer = 0
    },

    startTimer() {
      this.timerInterval = setInterval(() => {
        this.timer++
      }, 1000)
    },

    getRandomFinishDuration: () => Math.random() * 10 + 5,

    getRandomEase() {
      return this.eases[Math.floor(Math.random() * this.eases.length)]
    },

    placeBet(bet) {
      return !this.isTweening ? (this.userBet = bet) : false
    },

    async completeRace({ snail_id, message }) {
      let win = this.userBet === snail_id
      clearInterval(this.timerInterval)

      if (this.heirarchy === 0) {
        Swal.fire({
          title: win ? 'Congratulations' : 'You Lost',
          text: win
            ? `Your snail won ${Math.floor(Math.random() * 500)}!`
            : 'Try again!',
          icon: win ? 'info' : 'question',
          confirmButtonText: 'Cool'
        })
      }

      this.userBet = null
      this.heirarchy++
    },

    start() {
      this.reset()

      if (this.userBet === null) {
        return Swal.fire({
          title: 'Oops!',
          text: 'Please place your bet first!',
          icon: 'error',
          confirmButtonText: 'Okay'
        })
      }

      let obj = [
        {
          element: '.snail-1',
          params: [
            { message: 'Snail One!', snail_id: 1, ease: this.getRandomEase() }
          ]
        },
        {
          element: '.snail-2',
          params: [
            { message: 'Snail Two!', snail_id: 2, ease: this.getRandomEase() }
          ]
        },
        {
          element: '.snail-3',
          params: [
            { message: 'Snail Three!', snail_id: 3, ease: this.getRandomEase() }
          ]
        }
      ]

      this.startTimer()
      obj.forEach(({ element, params }) =>
        this.tween.to(element, {
          duration: this.getRandomFinishDuration(),
          x: this.finishLine,
          ease: this.getRandomEase(),
          onComplete: this.completeRace,
          onCompleteParams: params
        })
      )
    }
  }
}
</script>

<style lang="scss" scoped>
// Mixins
@mixin snail-item($url) {
  background-image: url($url);
  background-size: contain;
  background-repeat: no-repeat;
}

@mixin padding($top, $right: $top, $bottom: $top, $left: $top) {
  padding-top: $top;
  padding-right: $right;
  padding-bottom: $bottom;
  padding-left: $left;
}

@mixin border-background($red, $green, $blue) {
  border: 1px solid rgba($red, $green, $blue, 1);
  background: rgba($red, $green, $blue, 0.7);
}

// Styles
.race-ground {
  background: rgba(250, 211, 144, 1);
  @include padding(1%, 1%, 5%, 1%);

  .lane {
    height: 100px;
    line-height: 100px;
    background: rgba(44, 62, 80, 1);
    margin: 5px;
    @include padding(5px, 0, 5px, 0);

    .snail {
      height: 98px;
      width: 150px;
      line-height: 98px;
      text-align: center;
      background-position: center;

      &.snail-1 { @include snail-item('../assets/car1.png'); }
      &.snail-2 { @include snail-item('../assets/car2.png'); }
      &.snail-3 { @include snail-item('../assets/car3.png'); }
    }
  }
}

.light-container {
  display: flex;
  justify-content: center;

  .light {
    width: 40px;
    text-align: center;
    height: 40px;
    background: #000;
    margin: 2px;
    border-radius: 50%;

    &.light-3 {
      background: red;
    }

    &.light-2 {
      background: orange;
    }

    &.light-1 {
      background: green;
    }
  }
}

.active {
  background-color: rgba(44, 62, 80, 0.4) !important;
  border-color: rgba(44, 62, 80, 0.7) !important;
}

.title-bar {
  display: flex;
  justify-content: space-between;
}

.layout {
  margin-left: 10%;
  margin-right: 10%;
  margin-top: 10%;
}

.bet-container {
  display: flex;

  .bet {
    width: 100%;
    height: 50px;
    line-height: 50px;
    margin: 5px;
    text-align: center;
    font-size: 1.5rem;
    cursor: pointer;
    position: relative;

    .bet-img {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }

    &.bet-1 { @include border-background(229, 142, 38) }

    &.bet-2 { @include border-background(74, 105, 189) }

    &.bet-3 { @include border-background(229, 80, 57) }
  }
}

.btn {
  width: 100%;
  border-radius: 5px;
  cursor: pointer;
  @include padding(10px);

  &.btn-start {
    border: 1px solid rgba(12, 36, 97, 1);
    background: rgba(12, 36, 97, 0.7);
    color: white;
  }
}
</style>
