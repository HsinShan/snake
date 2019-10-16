<template>
  <div class="snake">
    <h2>Score: {{score}}</h2>
    <svg
      version="1.1"
      baseProfile="full"
      width="500"
      height="500"
      xmlns="http://www.w3.org/2000/svg"
      class="land"
    >
      <rect width="100%" height="100%" fill="#d8cdbd" />
      <circle
        v-for="(coord, i) in coordinates"
        :key="i"
        :cx="coord.x"
        :cy="coord.y"
        r="10"
        fill="#999"
      />
      <circle
        :cx="food.x"
        :cy="food.y"
        r="10"
        fill="red"
      />
    </svg>
  </div>
</template>

<script>
export default {
  name: 'Snake',
  data () {
    return {
      coordinates: [
        {
          x: 10,
          y: 10
        },
        {
          x: 30,
          y: 10
        },
        {
          x: 50,
          y: 10
        }
      ],
      direction: 'right',
      head: null,
      body: null,
      food: {
        x: 0,
        y: 0
      },
      score: 0,
      isEatting: false,
      timer: null
    }
  },
  methods: {
    startGame () {
      this.timer = setInterval(() => {
        if (!this.isEatting) this.coordinates.shift()
        this.isEatting = false

        if (this.direction === 'right') {
          return this.goRight()
        }
        if (this.direction === 'down') {
          return this.goDown()
        }
        if (this.direction === 'up') {
          return this.goUp()
        }
        return this.goLeft()
      }, 300)
    },
    goLeft () {
      this.coordinates.push({
        x: this.head.x - 20,
        y: this.head.y
      })
    },
    goRight () {
      this.coordinates.push({
        x: this.head.x + 20,
        y: this.head.y
      })
    },
    goUp () {
      this.coordinates.push({
        x: this.head.x,
        y: this.head.y - 20
      })
    },
    goDown () {
      this.coordinates.push({
        x: this.head.x,
        y: this.head.y + 20
      })
    },
    createFood () {
      this.food.x = (Math.floor(Math.random() * 25) * 2 + 1) * 10
      this.food.y = (Math.floor(Math.random() * 25) * 2 + 1) * 10
      // 防止食物出現在 snake 身上
      this.coordinates.find(item => {
        if (JSON.stringify(item) === JSON.stringify(this.food)) {
          return this.createFood()
        }
      })
    },
    gameOver () {
      clearInterval(this.timer)
      this.timer = null
    }
  },
  mounted () {
    this.head = this.coordinates.slice(-1)[0]
    this.startGame()
    this.createFood()
    window.addEventListener('keydown', e => {
      switch (e.keyCode) {
        case 37:
          if (this.direction !== 'right') this.direction = 'left'
          break
        case 38:
          if (this.direction !== 'down') this.direction = 'up'
          break
        case 39:
          if (this.direction !== 'left') this.direction = 'right'
          break
        case 40:
          if (this.direction !== 'up') this.direction = 'down'
          break
      }
    })
  },
  watch: {
    coordinates () {
      this.head = this.coordinates.slice(-1)[0]
      this.body = this.coordinates.slice(0, -1)
      this.body.find(item => {
        if (JSON.stringify(item) === JSON.stringify(this.head)) {
          return this.gameOver()
        }
      })
      if (this.head.x < 0 || this.head.x > 500 || this.head.y < 0 || this.head.y > 500) {
        return this.gameOver()
      }

      if (JSON.stringify(this.head) === JSON.stringify(this.food)) {
        this.score += 1
        this.isEatting = true
        this.createFood()
      }
    }
  },
  destroyed () {
    window.removeEventListener('keydown', this.direction)
  }
}
</script>

<style lang="scss" scoped>
.land {
  border: 5px solid;
  border-top-color: #9c753e;
  border-bottom-color: #9c753e;
  border-left-color: #b19b7a;
  border-right-color: #b19b7a;
}
</style>
