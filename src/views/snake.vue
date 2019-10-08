<template>
  <div class="snake">
    <svg
      version="1.1"
      baseProfile="full"
      width="500"
      height="500"
      xmlns="http://www.w3.org/2000/svg"
    >
      <rect width="100%" height="100%" fill="#eee" />
      <circle
        v-for="(coord, i) in coordinates"
        :key="i"
        :cx="coord.x"
        :cy="coord.y"
        r="10"
        fill="#999"
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
      direction: 'right'
    }
  },
  methods: {
    startGame () {
      let timer = null
      let head = null
      timer = setInterval(() => {
        head = this.coordinates.slice(-1)[0]
        this.coordinates.shift()
        if (this.direction === 'right') {
          return this.goRight(head)
        }
        if (this.direction === 'down') {
          return this.goDown(head)
        }
        if (this.direction === 'up') {
          return this.goUp(head)
        }
        return this.goLeft(head)
      }, 1000)
      return timer
    },
    goLeft (head) {
      this.coordinates.push({
        x: head.x - 20,
        y: head.y
      })
    },
    goRight (head) {
      this.coordinates.push({
        x: head.x + 20,
        y: head.y
      })
    },
    goUp (head) {
      this.coordinates.push({
        x: head.x,
        y: head.y - 20
      })
    },
    goDown (head) {
      this.coordinates.push({
        x: head.x,
        y: head.y + 20
      })
    }
  },
  mounted () {
    this.startGame()
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
  destroyed () {
    window.removeEventListener('keydown', this.direction)
  }
}
</script>

<style scoped>
</style>
