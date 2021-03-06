<template>
  <transition name="fade" appear>
    <div
      class="wrapper"
      :class="isOK || isRouter ? 'light' : 'dark'"
      @click="goHome"
    >
      <div class="message" :class="{ out: isOK }">
        <div class="code">
          <span style="font-weight: 600">{{ code + " " }}</span>
          <transition name="fade">
            <span v-if="showTip" class="tip">{{ tip }}</span>
          </transition>
        </div>
        <div v-if="!isOK && msgList.length > 0">
          <div v-for="(msg, key) in msgList" :key="msg + key">{{ msg }}</div>
        </div>
        <div v-if="!isOK && !isRouter" class="message-animation">
          <div class="code">
            <span style="font-weight: 600">
              {{ code + " " }}
            </span>
            <span class="tip">{{ tip }}</span>
          </div>
          <div v-if="msgList.length > 0">
            <div v-for="(msg, key) in msgList" :key="msg + key">{{ msg }}</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import gsap from 'gsap'

export default {
  data: () => ({
    custom: false,
    number: null,
    tweenedNumber: 200,
    tip: '',
    codeList: [
      {
        code: 200,
        tip: 'OK'
      },
      {
        code: 400,
        tip: 'Bad Request'
      },
      {
        code: 401,
        tip: 'Unauthorized'
      },
      {
        code: 403,
        tip: 'Forbidden'
      },
      {
        code: 404,
        tip: 'Not Found'
      },
      {
        code: 405,
        tip: 'Method Not Allowed'
      },
      {
        code: 406,
        tip: 'Not Acceptable'
      },
      {
        code: 408,
        tip: 'Request Time-out'
      },
      {
        code: 401,
        tip: 'Conflict'
      },
      {
        code: 410,
        tip: 'Gone'
      },
      {
        code: 411,
        tip: 'Length Required'
      },
      {
        code: 412,
        tip: 'Precondition Failed'
      },
      {
        code: 413,
        tip: 'Request Entity Too Large'
      },
      {
        code: 414,
        tip: 'Request-URI Too Large'
      },
      {
        code: 415,
        tip: 'Unsupported Media Type'
      },
      {
        code: 416,
        tip: 'Requested range not satisfiable'
      },
      {
        code: 500,
        tip: 'Internal Server Error'
      },
      {
        code: 501,
        tip: 'Not Implemented'
      },
      {
        code: 502,
        tip: 'Bad Gateway'
      },
      {
        code: 503,
        tip: 'Service Unavailable'
      },
      {
        code: 504,
        tip: 'Gateway Time-out'
      },
      {
        code: 505,
        tip: 'HTTP Version not supported'
      }
    ],
    msgList: [],
    isOK: false,
    isRouter: false,
    showTip: true
  }),
  methods: {
    goHome() {
      if (this.isOK || this.isRouter) {
        this.$router.push('/index')
      } else {
        if (!this.custom) {
          clearInterval(this.timer)
          this.number = 200
          this.tip = 'OK'
        }
        this.isOK = true
        this.timer = setTimeout(() => {
          this.$router.push('/index')
        }, 1400)
      }
    },
    async getCode() {
      const index = this.getrRandomInt(this.codeList.length - 1)
      if (this.number !== this.codeList[index].code) {
        this.number = this.codeList[index].code
        this.showTip = false
        await this.$nextTick()
        this.showTip = true
        this.tip = this.codeList[index].tip
      } else {
        this.getCode()
      }
    },
    setTimer() {
      this.timer = setTimeout(async () => {
        await this.getCode()
        this.setTimer()
      }, this.getrRandomInt(400, 1000))
    },
    getrRandomInt(x, y) {
      if (arguments.length === 1) {
        return Math.floor(Math.random() * Math.floor(x))
      } else if (arguments.length === 2) {
        return Math.floor(Math.random() * (y - x + 1) + x)
      }
    }
  },
  computed: {
    code() {
      return this.tweenedNumber.toFixed(0)
    }
  },
  watch: {
    number(newValue) {
      if (!this.custom) {
        if (newValue === 200) {
          this.isRouter = true
          document.title = '200 OK'
        } else {
          this.isRouter = false
          document.title = `${newValue} ERROR?`
        }
      }
      gsap.to(this.$data, { duration: 0.5, tweenedNumber: newValue })
    }
  },
  mounted() {
    this.custom = this.$route.meta.code && this.codeList.some(item => item.code === this.$route.meta.code)
    if (this.custom) {
      const temp = this.codeList.find(item => item.code === this.$route.meta.code)
      this.number = temp.code
      this.tip = temp.tip
      document.title = `${temp.code} ${temp.tip}`
    } else {
      this.getCode()
      this.setTimer()
      document.onvisibilitychange = () => {
        if (document.visibilityState === 'hidden') {
          clearInterval(this.timer)
          document.title = '200 OK'
        } else {
          this.getCode()
          this.setTimer()
        }
      }
    }
  },
  beforeDestroy() {
    clearInterval(this.timer)
    document.onvisibilitychange = null
  }
}
</script>

<style lang="stylus" scoped>
@keyframes glitch
  $indent = 100

  for $index in 1 .. $indent
    {$index * 100 / $indent + '%'}
      clip-path inset(random(0, 100) + '%' 0 random(0, 100) + '%' 0)
      transform translate(random(-10, 10) * 1px, random(-10, 10) * 1px)

flex()
  display flex
  justify-content center
  align-items center
  flex-direction column

.dark
  background #000
  color #fff

.light
  background #fff
  color #000

.out
  overflow hidden
  animation message-out 0.5s
  animation-delay 800ms
  animation-fill-mode forwards

@keyframes message-out
  from
    max-height 100vh

  to
    max-height 0

.wrapper
  flex()
  overflow hidden
  width 100vw
  height 100vh
  user-select none
  transition background 0.2s, color 0.2s

  .message
    flex()
    width 99%
    position relative
    text-align left
    font-size 20px
    background transparent

    .message-animation
      content ''
      flex()
      width 99%
      position absolute
      top 0
      right 0
      bottom 0
      left 0
      background transparent
      color rgba(255, 255, 255, 0.5)
      text-shadow -3px -3px 0px rgba(248, 240, 5, 0.5), 3px 3px 0px rgba(0, 230, 246, 0.5)
      clip-path inset(50% 50% 50% 50%)
      animation glitch 25s infinite
      animation-timing-function steps(2, end)

.code
  font-size 50px
  margin-bottom 5px

  .tip
    font-size 30px

@media only screen and (max-width 500px)
  .code
    flex()

    .tip
      text-align center
</style>
