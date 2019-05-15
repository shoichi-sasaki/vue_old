<template>
    <div class="main">
    <b-modal :active.sync="startFlg"
             :canCancel="false">
      <button class="button is-info is-large" @click="startFlg=false; start()">Game Start</button>
    </b-modal>
    <b-modal :active.sync="gameOver"
             :canCancel="false">
      <h2>Replay?</h2>
      <button class="button is-info is-large" @click="gameOver=false; start()">Game Replay</button>
    </b-modal>
    <div class="columns">
      <div class="column" style="padding: 50px;">
        <div class="card" style="padding: 30px; height: 100px;">
          <h1>SCORE</h1>
          <p>{{score}}</p>
        </div>
      </div>
      <div class="column" style="padding-bottom: 0;">
        <table>
          <tbody>
            <tr v-for="(v, idxV) in VERTICAL" :key="idxV">
              <td v-for="(h, idxH) in HORIZONTAL" :key="idxH"
                  colspan="1">
                  <span :class="{'is-not-block' : gameTable[idxV][idxH] <= 0, 'is-block': gameTable[idxV][idxH] === 1 || gameTable[idxV][idxH] === 2}">
                  </span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Main',
  data () {
    return {
      picked1: false,
      picked2: true,
      VERTICAL: 20,
      HORIZONTAL: 10,
      score: 0,
      gameOver: false,
      startFlg: true,
      interval: null,
      isAction: false,
      isNext: false,
      level: 1000,
      block: [
        {x: 0, y: -1},
        {x: 0, y: -1},
        {x: 0, y: -1},
        {x: 0, y: -1}
      ],
      gameTable: [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
      ],
      initTable: [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
      ]
    }
  },
  methods: {
    startInit () {
      for (var y = 0; y < this.gameTable.length; y++) {
        for (var x = 0; x < this.HORIZONTAL; x++) {
          this.gameTable[y].splice(x, 1, 0)
        }
      }
    },
    start () {
      this.startInit()
      this.nextBlock()
      this.interval = setInterval(this.fall, this.level)
    },
    init () {
      console.log('*** init ***')
      console.log('=================')
      console.log(this.gameTable.length)
      console.log(this.gameTable[0].length)
      var count = 0
      for (var y = 0; y < this.gameTable.length; y++) {
        for (var x = 0; x < this.gameTable[y].length; x++) {
          if (this.gameTable[y][x] < 2) {
            this.gameTable[y].splice(x, 1, 0)
          }
          if (this.gameTable[y][x] === 1) console.log('!!! CHECK !!!\n' + this.gameTable[y][x])
          count++
        }
      }
      // this.gameTable = this.initTable
      // console.log('***** ' + count + ' *****')
      console.log('=================')
    },
    checkGameOver() {
      for (var y = 0; y < this.gameTable[y].length; y++) {
        for (var x = 0; x < this.HORIZONTAL; x++) {
          if (this.gameTable[0][x] === 2) {
            console.log('** game over **')
            this.$set(this, 'gameOver', true)
            clearInterval(this.interval)
          }
        }
      }
    },
    setBlock () {
      this.init()
      for (var b in this.block) {
        console.log('setBlock: ' + this.block[b].x + ', ' + this.block[b].y)
        this.gameTable[this.block[b].y].splice(this.block[b].x, 1, 2)
      }
    },
    draw () {
      this.init()
      for (var b in this.block) {
        console.log('draw b: ' + this.block[b].x + ', ' + this.block[b].y + ', ' + this.gameTable[this.block[b].y][this.block[b].x])
        if (this.gameTable[this.block[b].y][this.block[b].x] === 0) {
          console.log('draw: ' + this.block[b].x + ', ' + this.block[b].y)
          this.gameTable[this.block[b].y].splice(this.block[b].x, 1, 1)
        }
      }
    },
    actionKey (key) {
      console.log('Action')
      var isRightStop = false
      var isLeftStop = false
      var isDownStop = false
      var isRotateStop = false
      var maxX = this.block.map(function (p) { return p.x })
      var maxY = this.block.map(function (p) { return p.y })
      this.isAction = true
      for (var b1 in this.block) {
        console.log('check1: ' + this.block[b1].x + ', ' + this.block[b1].y)
        if (this.gameTable[this.block[b1].y][this.block[b1].x + 1] === 2 ||
            Math.max(...maxX) === this.HORIZONTAL - 1
            ) {
          isRightStop = true
        }
        if (this.gameTable[this.block[b1].y][this.block[b1].x - 1] === 2 ||
            Math.min(...maxX) === 0
            ) {
          isLeftStop = true
        }
        if (this.gameTable[this.block[b1].y + 1][this.block[b1].x] === 2 ||
            Math.max(...maxY) + 1 === this.VERTICAL
            ) {
          isDownStop = true
        }
        if (this.gameTable[Math.max(...maxY) + 1][this.block[b1].x] === 2 ||
            Math.max(...maxY) + 1 === this.VERTICAL
            ) {
          isRotateStop = true
        }
      }
      if (key === 'right') {
        if (!isRightStop) {
          this.move(1, 0)
          this.draw()
        }
      } else if (key === 'left') {
        if (!isLeftStop) {
          this.move(-1, 0)
          this.draw()
        }
      } else if (key === 'down') {
        if (!isDownStop) {
          this.move(0, 1)
          this.draw()
        }
      } else if (key === 'rotate') {
        if (!isRotateStop) {
          this.rotate(90)
          this.draw()
        } 
      }
      this.isAction = false
    },
    move (x, y) {
      this.init()
      for (var b in this.block) {
        this.block.splice(b, 1, {x: this.block[b].x + x, y: this.block[b].y + y})
      }
    },
    rotate (r) {
      var center = {x: 0, y: 0}
      center.x = this.block[1].x
      center.y = this.block[1].y
      var isStopRotate = false
      var rotateTable = []
      for (var b in this.block) {
        var y = (this.block[b].y - center.y) * Math.floor(Math.cos(Math.PI / 2)) + (this.block[b].x - center.x) * Math.floor(Math.sin(Math.PI / 2)) + center.y
		    var x = (this.block[b].x - center.x) * Math.floor(Math.cos(Math.PI / 2)) - (this.block[b].y - center.y) * Math.floor(Math.sin(Math.PI / 2)) + center.x
        if (x >= 0 && x <= this.HORIZONTAL - 1 && y >= 0 && y <= this.VERTICAL - 1) {
          rotateTable.push({x: x , y: y})
        } else {
          isStopRotate = true
        }
      }
      if (!isStopRotate) {
        for (var b in this.block) {
          this.block.splice(b, 1, rotateTable[b])
          console.log('actionRotate aft: ' + this.block[b].x + ', ' + this.block[b].y)
        }
      }
    },
    check () {
      var hitFlg = false
      var maxY = this.block.map(function (p) { return p.y })
      var maxX = this.block.map(function (p) { return p.x })
      for (var b1 in this.block) {
        console.log('check1: ' + this.block[b1].x + ', ' + this.block[b1].y)
        if (Math.max(...maxY) + 1 === this.VERTICAL) {
          hitFlg = true
          console.log('hit wall: ' + hitFlg)
          return 'next'
        } else if (this.gameTable[this.block[b1].y + 1][this.block[b1].x] === 2) {
          hitFlg = true
          console.log('hit block: ' + hitFlg + ', ' + (Math.max(...maxY) + 1) + ', ' + this.block[b1].x)
          return 'next'
        }
      }
      return 'playing'
    },
    arrayCheck (a, b) {
      for (var i = 0; i < a.length; i++) {
        if (a[i] !== b[i]) return false
      }
      return true
    },
    clearLine () {
      var line = [2, 2, 2, 2, 2, 2, 2, 2, 2, 2]
      var clearLine = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
      for (var y = 0; y < this.gameTable.length; y++) {
        if (this.arrayCheck(this.gameTable[y], line)) {
          this.gameTable.splice(y, 1)
          this.gameTable.unshift(clearLine)
          this.score += 100
          this.level -= 5
        }
      }
    },
    nextBlock () {
      switch (Math.floor(Math.random() * 7)) {
      // switch (0) {
        case 0:
          // this.block.splice(0, 1, {x: 5, y: 0})
          // this.block.splice(1, 1, {x: 5, y: 1})
          // this.block.splice(2, 1, {x: 5, y: 2})
          // this.block.splice(3, 1, {x: 5, y: 3})
          this.block = [
            {x: 5, y: 0},
            {x: 5, y: 1},
            {x: 5, y: 2},
            {x: 5, y: 3}
          ]
          break
        case 1:
          this.block.splice(0, 1, {x: 5, y: 0})
          this.block.splice(1, 1, {x: 5, y: 1})
          this.block.splice(2, 1, {x: 4, y: 0})
          this.block.splice(3, 1, {x: 4, y: 1})
          break
        case 2:
          this.block.splice(0, 1, {x: 5, y: 0})
          this.block.splice(1, 1, {x: 5, y: 1})
          this.block.splice(2, 1, {x: 5, y: 2})
          this.block.splice(3, 1, {x: 4, y: 2})
          break
        case 3:
          this.block.splice(0, 1, {x: 5, y: 0})
          this.block.splice(1, 1, {x: 5, y: 1})
          this.block.splice(2, 1, {x: 5, y: 2})
          this.block.splice(3, 1, {x: 6, y: 2})
          break
        case 4:
          this.block.splice(0, 1, {x: 5, y: 0})
          this.block.splice(1, 1, {x: 5, y: 1})
          this.block.splice(2, 1, {x: 4, y: 1})
          this.block.splice(3, 1, {x: 6, y: 1})
          break
        case 5:
          this.block.splice(0, 1, {x: 5, y: 0})
          this.block.splice(1, 1, {x: 5, y: 1})
          this.block.splice(2, 1, {x: 4, y: 1})
          this.block.splice(3, 1, {x: 4, y: 2})
          break
        case 6:
          this.block.splice(0, 1, {x: 5, y: 0})
          this.block.splice(1, 1, {x: 5, y: 1})
          this.block.splice(2, 1, {x: 6, y: 1})
          this.block.splice(3, 1, {x: 6, y: 2})
          break
      }
    },
    fall () {
      if (this.check() === 'playing') {
        if (!this.isAction && !this.isNext) {
          this.move(0, 1)
          this.draw()
        }
      } else {
        this.isNext = true
        this.setBlock()
        this.clearLine()
        this.nextBlock()
        this.isNext = false
        this.isAction = false
      }
      this.checkGameOver()
    },
    del(){
        this.picked1 = false;
        this.picked2 = true;
    }
  },
  mounted () {
    var vm = this
    document.onkeydown = function (e) {
      if (e.keyCode === 37) vm.actionKey('left')
      else if (e.keyCode === 32) vm.actionKey('rotate')
      else if (e.keyCode === 39) vm.actionKey('right')
      else if (e.keyCode === 40) vm.actionKey('down')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h2 {
  color: white;
}
table {
  height: 100vh;
}
td {
  width: 5vh;
  height: 5vh;
  background-color:#414868;
}
.is-not-block {
  background-color:#414868;
  width: 100%;
  height: 100%;
}
.is-block {
  background-color:#6375DC;
  wwidth: 100%;
  height: 100%;
  border: solid thin #eee;
}
</style>
