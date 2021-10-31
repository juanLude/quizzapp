<template>
  <div class="card text-center">
  <div class="card-header">
    Results
  </div>
  <div class="card-body">
    <h5 class="card-title">You got {{ (numCorrect/numTotal)*100 }}% of the answers correct</h5>
    <p class="card-text">If you feel like doing another one, please click the button below</p>
    <a @click="refresh" class="btn btn-primary">Click Me</a>
  </div>
  <div class="card-footer text-muted">
    It took you {{ (time/60).toFixed(2) }} minutes to do it.
  </div>
</div>
</template>

<script>

export default {
    props: [ 'numCorrect', 'numTotal', 'time', 'interval'],
    methods: {
        refresh() { 
            window.location.reload()
        },
      stopTimer() {
        clearInterval(this.interval)
      }
    },
    mounted() {
      this.stopTimer()
    }
}
</script>

<style>

</style>