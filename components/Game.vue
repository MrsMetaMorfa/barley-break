<template>
  <div class="game">
    <div class="game_header">
      <p class="game_steps-counter">Steps <span>{{ this.stepsCount }}</span></p>
      <p class="game_timer">Time <span class="time">
        <span>{{ (this.timer.minutes.toString().length === 1) ? ('0' + this.timer.minutes) : this.timer.minutes}}</span>
        :
        <span>{{ (this.timer.seconds.toString().length === 1) ? ('0' + this.timer.seconds) : this.timer.seconds}}</span>
      </span></p>
      <button class="game_restart button--green" type="button" @click="restartGame">Restart game</button>
    </div>
    <div class="game_container">
      <div class="game_container--inner">
        <tile v-for="(tile, index, key) in tiles"
              :key="key"
              :index="index"
              :tile="tile"
              :activeTile.sync="activeTile"
        />
      </div>
      <div class="game_win" :class="{ visible: isSolved }" v-if="isSolved">
        <p class="title">You win!</p>
      </div>
    </div>
  </div>
</template>

<script>
  import Tile from "./Tile";

  export default {
      name: 'Game',
      components: {
          Tile
      },
      data () {
          return {
              isSolved: false,
              stepsCount: 0,
              timer: {
                  isActive: true,
                  minutes: 0,
                  seconds: 0
              },
              activeTile: {},
              tiles: {
                0: {
                    value: 1,
                    positionX: 0,
                    positionY: 0,
                    positionXDefault: 0,
                    positionYDefault: 0,
                    isEmpty: false
                },
                1: {
                    value: 2,
                    positionX: 1,
                    positionY: 0,
                    positionXDefault: 1,
                    positionYDefault: 0,
                    isEmpty: false
                },
                2: {
                    value: 3,
                    positionX: 2,
                    positionY: 0,
                    positionXDefault: 2,
                    positionYDefault: 0,
                    isEmpty: false
                },
                3: {
                    value: 4,
                    positionX: 3,
                    positionY: 0,
                    positionXDefault: 3,
                    positionYDefault: 0,
                    isEmpty: false
                },
                4: {
                    value: 5,
                    positionX: 0,
                    positionY: 1,
                    positionXDefault: 0,
                    positionYDefault: 1,
                    isEmpty: false
                },
                5: {
                    value: 6,
                    positionX: 1,
                    positionY: 1,
                    positionXDefault: 1,
                    positionYDefault: 1,
                    isEmpty: false
                },
                6: {
                    value: 7,
                    positionX: 2,
                    positionY: 1,
                    positionXDefault: 2,
                    positionYDefault: 1,
                    isEmpty: false
                },
                7: {
                    value: 8,
                    positionX: 3,
                    positionY: 1,
                    positionXDefault: 3,
                    positionYDefault: 1,
                    isEmpty: false
                },
                8: {
                    value: 9,
                    positionX: 0,
                    positionY: 2,
                    positionXDefault: 0,
                    positionYDefault: 2,
                    isEmpty: false
                },
                9: {
                    value: 10,
                    positionX: 1,
                    positionY: 2,
                    positionXDefault: 1,
                    positionYDefault: 2,
                    isEmpty: false
                },
                10: {
                    value: 11,
                    positionX: 2,
                    positionY: 2,
                    positionXDefault: 2,
                    positionYDefault: 2,
                    isEmpty: false
                },
                11: {
                    value: 12,
                    positionX: 3,
                    positionY: 2,
                    positionXDefault: 3,
                    positionYDefault: 2,
                    isEmpty: false
                },
                12: {
                    value: 13,
                    positionX: 0,
                    positionY: 3,
                    positionXDefault: 0,
                    positionYDefault: 3,
                    isEmpty: false
                },
                13: {
                    value: 14,
                    positionX: 1,
                    positionY: 3,
                    positionXDefault: 1,
                    positionYDefault: 3,
                    isEmpty: false
                },
                14: {
                    value: 15,
                    positionX: 2,
                    positionY: 3,
                    positionXDefault: 2,
                    positionYDefault: 3,
                    isEmpty: false
                },
                15: {
                    value: '',
                    positionX: 3,
                    positionY: 3,
                    positionXDefault: 3,
                    positionYDefault: 3,
                    isEmpty: true
                },
            },
              defaultTilesPosition: [
                  {x: 0, y: 0}, {x: 1, y: 0}, {x: 2, y: 0}, {x: 3, y: 0},
                  {x: 0, y: 1}, {x: 1, y: 1}, {x: 2, y: 1}, {x: 3, y: 1},
                  {x: 0, y: 2}, {x: 1, y: 2}, {x: 2, y: 2}, {x: 3, y: 2},
                  {x: 0, y: 3}, {x: 1, y: 3}, {x: 2, y: 3}, {x: 3, y: 3}
              ]
          }
      },
      watch: {
          activeTile: function () {
            this.moveTile();
          }
      },
      created() {
        this.newGame();
      },
      methods: {
          newGame() {
              let newTilesPosition = this.defaultTilesPosition;
              newTilesPosition = this.defaultTilesPosition.sort(() => Math.random() - 0.5);
              do {
                  newTilesPosition = this.defaultTilesPosition.sort(() => Math.random() - 0.5);
              } while (this.isSolvable(newTilesPosition) !== true );
              for (let i = 0; i < newTilesPosition.length; i++) {
                  this.tiles[i].positionX = newTilesPosition[i].x;
                  this.tiles[i].positionY = newTilesPosition[i].y;
              }
              this.isSolved = false;
              this.stepsCount = 0;
              this.timer.isActive = true;
              this.timer.minutes = this.timer.seconds = 0;
              this.setTimer();
            },
          isSolvable(array) {
              let countInversions = 0;
              for (let i = 0; i < array.length; i++) {
                  for (let j = 0; j < i; j++) {
                      if (array[j] > array[i])
                          countInversions++;
                  }
              }
              return (countInversions % 2) == 0;
          },
          moveTile() {
              let emptyPosX = this.tiles["15"].positionX,
                  emptyPosY = this.tiles["15"].positionY;
              if ((this.activeTile.positionX - emptyPosX <= 1) &&
                  (this.activeTile.positionX - emptyPosX >= -1) &&
                  (this.activeTile.positionY - emptyPosY <= 1) &&
                  (this.activeTile.positionY - emptyPosY >= -1)) {
                  if ((this.activeTile.positionX - emptyPosX == 0) || (this.activeTile.positionY - emptyPosY == 0)) {
                      let index = this.activeTile.index;
                      this.tiles[index].positionX = emptyPosX;
                      this.tiles[index].positionY = emptyPosY;
                      this.tiles["15"].positionX = this.activeTile.positionX;
                      this.tiles["15"].positionY = this.activeTile.positionY;
                      this.stepsCount = this.stepsCount + 1;
                  }
              }
              if (this.isSolve()) {
                  this.isSolved = true;
                  this.timer.isActive = false;
                  this.setTimer();
              }
          },
          isSolve() {
              let rightPositions = [];
              for (let c = 0; c < this.defaultTilesPosition.length; c++) {
                  rightPositions.push(
                      this.tiles[c].positionX === this.defaultTilesPosition[c].x &&
                      this.tiles[c].positionY === this.defaultTilesPosition[c].y
                  );
              }

              if (rightPositions.every(elem => elem > 0)) {
                  return true;
              }
          },
          setTimer() {
              let timer = setTimeout(() => this.setTimer(), 1000);
              if (this.timer.isActive) {
                  this.timer.seconds = this.timer.seconds + 1;
                  if (this.timer.seconds === 60) {
                      this.timer.minutes = this.timer.minutes + 1;
                      this.timer.seconds = 0;
                  }
              } else {
                  clearTimeout(timer);
              }
          },
          restartGame() {
              this.timer.isActive = false;
              setTimeout(() => this.newGame(), 1500);
          }
      }
  }
</script>

<style lang="scss">
  .game {
    max-width: 420px;
    width: calc(100vw - 20px);
    padding: 20px 10px;
    &_container {
      max-height: 400px;
      height: calc(100vw - 20px);
      padding: 5px;
      margin: 10px;
      background: #35495e;
      position: relative;
      &--inner {
        width: 100%;
        height: 100%;
        position: relative;
      }
    }
    &_header {
      display: flex;
      align-items: center;
      justify-content: space-around;
    }
    &_steps-counter {
      display: block;
      text-align: center;
      text-transform: uppercase;
      font-size: 0.75em;
      line-height: 1;
      span {
        display: block;
        font-weight: 700;
        font-size: 1rem;
        line-height: 1.2;
      }
    }
    &_timer {
      text-align: center;
      text-transform: uppercase;
      font-size: 0.75em;
      line-height: 1;
      .time {
        display: block;
        font-weight: 700;
        font-size: 1rem;
        line-height: 1.2;
      }
    }
    &_restart {
      cursor: pointer;
      padding: 10px 10px 8px;
      text-transform: uppercase;
      font-size: 0.75rem;
      line-height: 1;
    }
    &_win {
      display: flex;
      position: absolute;
      z-index: 10;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(59, 128, 112, .75);
      visibility: hidden;
      opacity: 0;
      color: white;
      align-items: center;
      justify-content: center;
      font-size: 3rem;
      line-height: 1.2;
      font-weight: 700;
      transition: opacity ease-in-out 0.3s;
      &.visible {
        visibility: visible;
        opacity: 1;
      }
    }
  }
</style>

