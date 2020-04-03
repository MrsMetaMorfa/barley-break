<template>
  <button class="game_tile" type="button"
       :class="{'game_tile--empty': tile.isEmpty}"
       :style="{ left: (tile.positionX * 25) + '%', top: (tile.positionY * 25) + '%' }"
       @click="moveTile"
  >
    <span class="value">
      {{ tile.value }}
    </span>
  </button>
</template>

<script>
  export default {
      name: 'Tile',
      props: {
          index: {
              default: 0
          },
          tile: {
              value: {
                  type: String,
                  default: 0
              },
              positionX: {
                  type: Number,
                  default: 0
              },
              positionY: {
                  type: Number,
                  default: 0
              },
              positionXDefault: {
                  type: Number,
                  default: 0
              },
              positionYDefault: {
                  type: Number,
                  default: 0
              },
              isEmpty: {
                  type: Boolean,
                  default: false
              }
          }
      },
      methods: {
          moveTile() {
              if ( this.index !== '15' ) {
                  this.$emit('update:activeTile', {
                      index: this.index,
                      positionX: this.tile.positionX,
                      positionY: this.tile.positionY
                  });
              }
          }
      }
  }
</script>

<style lang="scss">
  .game_tile {
    width: 25%;
    height: 25%;
    padding: 5px;
    background: transparent;
    position: absolute;
    cursor: pointer;
    margin: 0;
    border: none;
    display: block;
    outline: none;
    transition: left ease-in-out 0.3s, top ease-in-out 0.3s;
    &--empty {
      opacity: 0;
    }
    .value {
      background: #3b8070;
      color: white;
      width: 100%;
      height: 100%;
      font-size: 2.4em;
      padding: 0;
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 25%;
      font-weight: 700;
    }
  }
</style>

