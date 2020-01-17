<template>
  <div class="parent-layout">
    <div class="layout">
      <div class="title-bar">
        <img src="@/assets/title.png" class="title--img" alt="" />
      </div>

      <div class="race--ground">
        <div class="ground ground--upper" />
        <div class="lane lane--1">
          <div class="snail snail--1" />
        </div>
        <div class="lane lane--2">
          <div class="snail snail--2" />
        </div>
        <div class="lane lane--3">
          <div class="snail snail--3" />
        </div>
        <div class="ground ground--lower" />
      </div>

      <div class="action-bar">
        <div class="bet-container action-bar__item">
          <div
            class="bet bet--1"
            :class="{ active: userBet === 1 }"
            @click="placeBet(1)"
          >
            <div class="bet__badge">1</div>
            <img
              src="@/assets/bet-car-1.png"
              class="bet__img"
              :width="userBet === 1 ? '30px' : '25px'"
            />
          </div>
          <div
            class="bet bet--2"
            :class="{ active: userBet === 2 }"
            @click="placeBet(2)"
          >
            <div class="bet__badge">1</div>
            <img
              src="@/assets/bet-car-2.png"
              class="bet__img"
              :width="userBet === 2 ? '30px' : '25px'"
            />
          </div>
          <div
            class="bet bet--3"
            :class="{ active: userBet === 3 }"
            @click="placeBet(3)"
          >
            <div class="bet__badge">1</div>
            <img
              src="@/assets/bet-car-3.png"
              class="bet__img"
              :width="userBet === 3 ? '30px' : '25px'"
            />
          </div>
        </div>

        <button
          class="btn"
          :class="{ 'btn--start': !isTweening, 'btn--disabled': isTweening }"
          @click="start"
        >
          START
        </button>
      </div>

      <div class="footer">
        <span class="footer__text footer__text--points">
          5, 000
        </span>
        <span class="footer__text">
          Lorem, ipsum dolor sit amet consectetur adipisicing elit
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { TweenMax, gsap } from 'gsap'
import Swal from 'sweetalert2'

export default {
  name: 'home',

  data: () => ({
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
    ]
  }),

  async mounted() {
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
      if (this.isTweening) return

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
          element: '.snail--1',
          params: [
            { message: 'Snail One!', snail_id: 1, ease: this.getRandomEase() }
          ]
        },
        {
          element: '.snail--2',
          params: [
            { message: 'Snail Two!', snail_id: 2, ease: this.getRandomEase() }
          ]
        },
        {
          element: '.snail--3',
          params: [
            { message: 'Snail Three!', snail_id: 3, ease: this.getRandomEase() }
          ]
        }
      ]

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
@import '../assets/scss/mixins';
@import '../assets/scss/race-ground';
@import '../assets/scss/action-bar';
@import '../assets/scss/vars';

.parent-layout {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  user-select: none !important;
}
.layout {
  margin-left: 10%;
  margin-right: 10%;
  margin-top: 5%;
  position: relative;
  max-width: 490px;
  background-size: contain;
  padding: 2%;
  background-color: #531789;
  background-image: url('../assets/bg-01.png');
  background-repeat: no-repeat;

  .title-bar {
    display: flex;
    justify-content: center;
    position: relative;

    .title--img {
      z-index: 2;
      height: 150px;
      margin-bottom: -30px;
      margin-top: 0;
      transition: all 1s linear 0s;

      @media screen and (max-width: 560px) {
        margin-bottom: -20px;
      }

      @media screen and (max-width: 420px) {
        height: 80px;
      }
    }
  }

  .footer {
    background: #3a0f54;
    border-radius: 10px;
    padding: 10px;

    .footer__text {
      color: #efe9f2;

      &.footer__text--points {
        color: rgb(252, 172, 1);
        font-weight: bold;
        letter-spacing: 1px;
        margin-right: 5px;
      }
    }
  }
}
</style>
