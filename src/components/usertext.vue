<template>
  <div id="uesrtext">
    <textarea
      placeholder="按 Ctrl + Enter 发送"
      v-model="content"
      v-on:keyup="addMessage"
    ></textarea>
  </div>
</template>

<script>
import { mapState } from 'vuex'

export default {
  name: 'uesrtext',
  data() {
    return {
      content: ''
    }
  },
  methods: {
    addMessage(e) {
      if (e.ctrlKey && e.keyCode === 13 && this.content.length) {
        const obj = new Object()
        obj.to = this.currentSession.username
        obj.content = this.content
        this.$store.state.stomp.send('/ws/chat', {}, JSON.stringify(obj))
        // 发给自己
        obj.notSelf = false
        this.$store.commit('addMessage', obj)
        this.content = ''
      }
    }
  },
  computed: mapState(['hrs', 'currentSession'])
}
</script>

<style lang="scss" scoped>
#uesrtext {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 30%;
  border-top: solid 1px #ddd;
  > textarea {
    padding: 10px;
    width: 100%;
    height: 100%;
    border: none;
    outline: none;
  }
}
</style>
