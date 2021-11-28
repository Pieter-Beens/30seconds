<template>
  <div class="timer">
    <Hourglass />
    {{ this.timeRemaining }}
  </div>
</template>


<script>
import Hourglass from './Hourglass.vue'

export default {
  name: 'Timer',
  components: { Hourglass },
  props: {
    timeAlotted: Number
  },
  data: function() {
    return {
        timeRemaining: this.timeAlotted + 0
    }
  },
  mounted() {
    setInterval(() => this.decrementTimer(), 1000);
  },
  methods: {
    decrementTimer() {
        if (this.timeRemaining > 0) this.timeRemaining--;
    },
    resetTimer() {
        this.timeRemaining = this.timeAlotted + 0;
    }
  }
}
</script>

<style scoped>
.timer {

}
</style>
