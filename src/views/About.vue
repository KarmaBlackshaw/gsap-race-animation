<template>
  <div>
    <svg xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 379 109" class="svg-button">
      <defs>
        <linearGradient id="linear-gradient" x1="189.5" y1="47.46" x2="189.5" y2="6.9" gradientUnits="userSpaceOnUse">
          <stop offset="0" stop-color="#fca601"/>
          <stop offset="1" stop-color="#febd01"/>
        </linearGradient>
      </defs>
      <title>button_text</title>
      <g id="Layer_2" data-name="Layer 2">
        <g id="Layer_2-2" data-name="Layer 2">
          <path class="cls-1" d="M379,23V95a14,14,0,0,1-14,14H14A14,14,0,0,1,0,95V23A14,14,0,0,1,14,9H365A14,14,0,0,1,379,23Z"/>
          <path class="cls-2" d="M379,14V86a14,14,0,0,1-14,14H14A14,14,0,0,1,0,86V14A14,14,0,0,1,14,0H365A14,14,0,0,1,379,14Z"/>
          <path class="cls-3" d="M374,14V86a9,9,0,0,1-9,9H14a9,9,0,0,1-9-9V14a9,9,0,0,1,9-9H365A9,9,0,0,1,374,14Z"/>
          <ellipse class="cls-4" cx="361.87" cy="8.19" rx="4.23" ry="12.95" transform="translate(289.38 362.83) rotate(-79.74)"/>
          <path class="cls-5" d="M379,30.52V93.8a6.2,6.2,0,0,1-6.2,6.2H14A14,14,0,0,1,0,86V72.23l5-1.39V86a9,9,0,0,0,9,9H370a4,4,0,0,0,4-4V29.51C375.68,29.83,377.35,30.17,379,30.52Z"/>
          <path class="cls-6" d="M374,29.51V86a9,9,0,0,1-9,9H14a9,9,0,0,1-9-9V70.84C71.52,52.49,256.2,6.84,374,29.51Z"/>
        </g>
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'name',

  data() {
    return {}
  },

  methods: {}
}
</script>

<style lang="scss">
.svg-button {
  .cls-1{fill:#eb6300;}
  .cls-2{fill:#fed500;}
  .cls-3{fill:url(#linear-gradient);}
  .cls-4{fill:#fff;}
  .cls-5, .cls-6{fill:#f99800;}
  .cls-5{opacity:0.6;}
  .cls-6{opacity:0.5;}
}
</style>