<template>
  <waterfall :data="data" style="margin: 5px 5px">
    <template v-slot:item="props">
      <Card class="card">
        <div slot="header">
          {{ props.item.title }}
        </div>
        <v-md-preview class="text" :text="props.item.text"></v-md-preview>
      </Card>
    </template>
  </waterfall>
</template>

<script>
import Waterfall from '@/components/common/Waterfall'
import Card from '@/components/common/Card'

export default {
  components: {
    Waterfall,
    Card
  },
  data() {
    return {
      data: [
        {
          id: 1,
          title: '这个主页',
          text: '[github](https://github.com/blacktunes/Xianyu-bbs)'
        },
        {
          id: 2,
          title: '🌶️神乐七奈的语音按钮',
          text: '[项目地址](https://kaguranana.moe/)\n[github](https://github.com/blacktunes/kaguranana-button)'
        },
        {
          id: 3,
          title: '🍣Hiiro的语音按钮',
          text: '[项目地址](https://hiiro.club/)\n[github](https://github.com/blacktunes/hiiro-button)'
        },
        {
          id: 4,
          title: '咸鱼Bot',
          text: '可使用HTTP API的QQ机器人框架\n[github](https://github.com/blacktunes/xianyu-robot)'
        },
        {
          id: 5,
          title: '仿微博WebApp',
          text: '已经鸽了很久了，择日重做\n[github](https://github.com/blacktunes/mood)'
        }
      ]
    }
  },
  methods: {
    back() {
      this.$router.push('/index')
    }
  }
}
</script>

<style lang="stylus" scoped>
a
  margin-left 5px
  color #888
  text-decoration none

.card
  display block
  font-size 18px
  text-align center
  margin 5px
  color #666

  .text
    box-sizing border-box
    font-size 16px
    padding 0

    & >>> code
      word-break normal
</style>
