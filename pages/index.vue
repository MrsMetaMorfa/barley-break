<template>
  <section class="wrapper">
    <div class="container">
      <game />
      <game />
    </div>
  </section>
</template>

<script>
import Game from '~/components/Game.vue'

export default {
  components: {
    Game
  }
}
</script>

<style lang="scss">
  .container {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    flex-wrap: wrap;
    padding: 3%;
  }
  .wrapper {
    width: 98%;
    max-width: 1920px;
    margin: 0 auto;
  }
</style>

