<template>
  <div class="example">
    <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 48 48" fill="none" aria-labelledby="menu" role="presentation">
      <line ref="top" x1="28" y1="18" x2="28" :y2="18"  stroke-width="2" stroke="currentColor"/>
    </svg>
</div>
</template>

<script>
import { mapState } from 'vuex'
import { TimelineLite} from 'gsap'

export default {
  name: 'AnimatedIconExample',

  data: () => ({
    timeline: new TimelineLite()
  }),

  computed: {
    ...mapState({
        isActive: state => state.ui.isActive
    })
  },

  watch: {
    isActive(newValue, oldValue){
      if(newValue){
        this.timeline.to( this.$refs.top, 0.3, { attr: {x1: 10, y1: 18, x2: 25, y2: 18 }})
        this.timeline.play()
      } else{
        this.timeline.reverse()
      }
    }
  }
}
