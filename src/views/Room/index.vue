<script setup lang="ts">
import RoomStatus from './components/RoomStatus.vue'
import RoomAction from './components/RoomAction.vue'
import RoomMessage from './components/RoomMessage.vue'
import io, { Socket } from 'socket.io-client'
import { onMounted, onUnmounted } from 'vue'
import { baseURL } from '@/utils/request'
import { useUserStore } from '@/stores'
import { useRoute } from 'vue-router'

const store = useUserStore()
const router = useRoute()
let socket: Socket
onMounted(() => {
  socket = io(baseURL, {
    auth: {
      token: `Bearer ${store.user?.token}`
    },
    query: {
      orderId: router.query.orderId
    }
  })
  socket.on('connect', () => {
    console.log('连接成功')
  })
  socket.on('disconnect', () => {
    console.log('连接关闭')
  })
  socket.on('error', () => {
    console.log('发生错误')
  })
})

onUnmounted(() => {
  socket.close()
})

// socket.on('connect', () => {
//   console.log('连接成功')
//   // 发送消息
//   socket.emit('chat message', '你好，sokcet.io')
// })

// socket.on('chat message', (msg) => {
//   console.log(msg)
//   socket.close()
// })

// socket.on('disconnect', () => {
//   console.log('连接关闭')
// })
</script>

<template>
  <div class="room-page">
    <cp-nav-bar title="问诊室"></cp-nav-bar>
    <!-- 状态栏 -->
    <room-status></room-status>
    <!-- 消息 -->
    <room-message></room-message>
    <!-- 操作栏 -->
    <room-action></room-action>
  </div>
</template>

<style lang="scss" scoped>
.room-page {
  padding-top: 90px;
  padding-bottom: 60px;
  min-height: 100vh;
  box-sizing: border-box;
  background-color: var(--cp-bg);
  .van-pull-refresh {
    width: 100%;
    min-height: calc(100vh - 150px);
  }
}
</style>
