<script setup lang="ts">
import { inject, type PropType } from 'vue'
import { useLikeToggle } from '@/hooks/useLikeToggle'
import { useChatStore } from '@/stores/chat'
import type { MessageType } from '@/services/types'

const props = defineProps({
  msg: {
    type: Object as PropType<MessageType>,
    required: true,
  },
})

const focusMsgInput = inject<() => void>('focusMsgInput')
const chatStore = useChatStore()

const { isLike, isDisLike, onLike, onDisLike } = useLikeToggle(props.msg.message)

/**
 * 回复消息
 */
const onReplyMsg = async (msgFromUser: MessageType) => {
  if (!msgFromUser) return
  chatStore.currentMsgReply = msgFromUser
  focusMsgInput?.()
}
</script>

<template>
  <div class="msg-option">
    <span class="msg-option-item" title="回复">
      <IconReply class="icon reply" v-login="() => onReplyMsg(msg)" />
    </span>
    <span class="msg-option-item" title="点赞">
      <IconLike :class="['icon', { 'like-active': isLike }]" v-login="() => onLike()" />
    </span>
    <span class="msg-option-item" title="不喜欢">
      <IconDislike :class="['icon', { 'dislike-active': isDisLike }]" v-login="() => onDisLike()" />
    </span>
    <!-- TODO -->
    <!-- <span class="msg-option-item" title="更多">
      <IconMore class="icon more" />
    </span> -->
  </div>
</template>

<style lang="scss" src="./styles.scss" scoped />
