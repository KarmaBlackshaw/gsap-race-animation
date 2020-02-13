<template>
  <div class="parent-layout">
    <div class="game-layout">
      <div class="game-layout--title-bar">
        <img src="@/assets/title.png" class="game-layout--title-bar__img" alt />
      </div>

      <div class="race-ground">
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
          <div class="bet bet--1" :class="{ 'active': userBet === 1 }" @click="placeBet(1)">
            <img
              src="@/assets/bet-car-1.png"
              class="bet__img"
            />
          </div>
          <div class="bet bet--2" :class="{ 'active': userBet === 2 }" @click="placeBet(2)">
            <img
              src="@/assets/bet-car-2.png"
              class="bet__img"
            />
          </div>
          <div class="bet bet--3" :class="{ 'active': userBet === 3 }" @click="placeBet(3)">
            <img
              src="@/assets/bet-car-3.png"
              class="bet__img"
            />
          </div>
        </div>

        <button
          class="btn"
          :class="isTweening ? 'btn--disabled' : 'btn--start'"
          @click="start"
        >
          <div class="winning-effect__text" id="winning-effect__text">
            <h1 class="stroke-inner">START</h1>
            <h1 class="stroke-inner-2">START</h1>
            <h1 class="no-stroke">START</h1>
          </div></button>
      </div>

      <div class="points-container">
        <span class="points-container__text-points">5,000</span>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Iusto vitae
      </div>
    </div>
  </div>
</template>

<script>
import { TweenMax, gsap } from "gsap";
import Swal from "sweetalert2";

export default {
  name: "home",

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
      "circ.inOut",
      "expo.inOut",
      "sine.inOut",
      "power1.inOut",
      "power2.inOut",
      "power3.inOut",
      "power4.inOut",
      "slow(0.3, 0.4, false)"
    ]
  }),

  mounted() {
    this.refreshSizes();
    window.addEventListener("resize", this.refreshSizes);
  },

  computed: {
    isTweening: vm => (vm.snails ? vm.tween.isTweening(vm.snails) : false)
  },

  methods: {
    refreshSizes() {
      this.lanes = null;
      this.snails = null;

      this.tween = gsap;
      this.lanes = document.getElementsByClassName("lane");
      this.snails = document.getElementsByClassName("snail");
      this.lane = this.lanes[0];
      this.snail = this.snails[0];
      this.laneWidth = this.lane.offsetWidth;
      this.snailWidth = this.snail.offsetWidth;
      this.finishLine = this.laneWidth - this.snailWidth;
    },

    reset() {
      this.heirarchy = 0;
      this.tween.set(this.snails, { x: 0 });
    },

    getRandomFinishDuration: () => Math.random() * 10 + 5,

    getRandomEase() {
      return this.eases[Math.floor(Math.random() * this.eases.length)];
    },

    placeBet(bet) {
      return !this.isTweening ? (this.userBet = bet) : false;
    },

    completeRace({ snail_id, message }) {
      let win = this.userBet === snail_id;

      if (this.heirarchy === 0) {
        Swal.fire({
          title: win ? "Congratulations" : "You Lost",
          text: win
            ? `Your snail won ${Math.floor(Math.random() * 500)}!`
            : "Try again!",
          icon: win ? "info" : "question",
          confirmButtonText: "Cool"
        });
      }

      this.userBet = null;
      this.heirarchy++;
    },

    start() {
      if (this.isTweening) return;

      this.reset();

      if (this.userBet === null) {
        return Swal.fire({
          title: "Oops!",
          text: "Please place your bet first!",
          icon: "error",
          confirmButtonText: "Okay"
        });
      }

      let obj = [
        {
          element: ".snail--1",
          params: [
            { message: "Snail One!", snail_id: 1, ease: this.getRandomEase() }
          ]
        },
        {
          element: ".snail--2",
          params: [
            { message: "Snail Two!", snail_id: 2, ease: this.getRandomEase() }
          ]
        },
        {
          element: ".snail--3",
          params: [
            { message: "Snail Three!", snail_id: 3, ease: this.getRandomEase() }
          ]
        }
      ];

      obj.forEach(({ element, params }) =>
        this.tween.to(element, {
          duration: this.getRandomFinishDuration(),
          x: this.finishLine,
          ease: this.getRandomEase(),
          onComplete: this.completeRace,
          onCompleteParams: params
        })
      );
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/mixins";
@import "../assets/scss/race-ground";
@import "../assets/scss/action-bar";
@import url("https://fonts.googleapis.com/css?family=Black+Han+Sans&display=swap");

.parent-layout {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  user-select: none !important;
}
.game-layout {
  margin-left: 10%;
  margin-right: 10%;
  position: relative;
  max-width: 490px;
  background-size: contain;
  padding: 2%;
  background-color: #531789;
  background-image: url("../assets/bg-01.png");
  background-repeat: no-repeat;

  .game-layout--title-bar {
    display: flex;
    justify-content: center;
    position: relative;

    .game-layout--title-bar__img {
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

  .points-container {
    background-color: #2e0a61;
    padding: 10px;
    border-radius: 10px;
    color: #e5dee7;
    font-size: 1.1rem;

    .points-container__text-points {
      color: #fdbc00;
      font-weight: 900;
      letter-spacing: 1px;
    }
  }
}
</style>
