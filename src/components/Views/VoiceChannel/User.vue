<script lang="ts" setup>
import type { PeerConnection } from '@/types'
import { ref, toRef, watch, watchEffect } from 'vue'
import MicrophoneIcon from "../../Icons/MicrophoneIcon.vue"
import MicrophoneOffIcon from '../../Icons/MicrophoneOffIcon.vue'

interface Props {
  peerConnection: PeerConnection
}
const props = defineProps<Props>()


const videoRef = ref<HTMLVideoElement | null>(null)
const audioRef = ref<HTMLAudioElement | null>(null)

const videoStreamRef = toRef(props.peerConnection, "ssVideoStream")
const audioStreamRef = toRef(props.peerConnection, "microphoneStream")

watch([videoRef, videoStreamRef], ([newVideo, newStream], [oldVideo, oldStream]) => {
  if (oldStream) {
  }
  console.log(audioStreamRef.value?.getTracks(),  videoStreamRef.value?.getTracks())
  if (newStream && videoRef.value) {
    videoRef.value.srcObject = newStream
  }
})

watch(() => props.peerConnection, (oldConnection, newConnection) => {
  if (newConnection) {
    console.log(newConnection.pc.getReceivers())
  }
})

watch([audioRef, audioStreamRef], ([newAudio, newStream], [oldAudio, oldStream]) => {
  if (oldStream) {

  }
  console.log(props.peerConnection.uuid)
  console.log(audioStreamRef.value?.getTracks(), videoStreamRef.value?.getTracks())
  if (newStream && audioRef.value) {
    audioRef.value.srcObject = newStream
  }
})

watchEffect(() => {
  console.log('effect')
})
</script>

<template>
  <div class="user-container">
    <div class="video-container" v-if="videoStreamRef">
      <video ref="videoRef" autoplay class="rtc-stream"></video>
    </div>
    <div v-else class="video-template">
      <img class="user-pic" src="https://e7.pngegg.com/pngimages/719/959/png-clipart-celebes-crested-macaque-monkey-selfie-grapher-people-for-the-ethical-treatment-of-animals-funny-mammal-animals-thumbnail.png"></img>
    </div>
    <div class="user-info">
      <div class="microphone-status">
        <MicrophoneIcon class="mic-icon"></MicrophoneIcon>
        <!-- <MicrophoneOffIcon class="mic-icon"></MicrophoneOffIcon> -->
      </div>
      <div class="display-name">
        {{ props.peerConnection.displayName }}
      </div>
    </div>
    <audio ref="audioRef" autoplay v-if="props.peerConnection.microphoneStream"></audio> 
  </div>
</template>

<style scoped></style>
