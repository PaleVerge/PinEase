<template>
  <div>
    <button @click="start">启动识别</button>
    <button @click="stop">停止</button>
    <div style="position:relative;">
      <video ref="videoRef" autoplay playsinline style="position:absolute;opacity:0"></video>
      <canvas ref="canvasRef"></canvas>
    </div>
  </div>
</template>
<script setup>
import {ref, onMounted} from 'vue'
import { PoseLandmarker, FilesetResolver } from '@mediapipe/tasks-vision';
const videoRef = ref(null)
const canvasRef = ref(null)
let videoEl = null
let canvasEl = null
let ctx = null
let poseLandmarker = null
let animationId = null
let stream = null
let vision = null
const POSE_CONNECTIONS = [
  [0,1],[1,2],[2,3],[3,4],[0,5],[5,6],[6,7],[7,8],
  [0,9],[9,10],[10,11],[11,12],[9,13],[13,14],[14,15],[15,16],
  [10,17],[17,18],[18,19],[19,20],[12,24],[24,23],[23,11],
  [12,23],[11,12],[24,25],[25,26],[26,27],[27,28],[28,32],[32,30],
  [28,31],[24,29],[29,30],[30,31],[27,31]
];
onMounted(()=>{
  videoEl = videoRef.value
  canvasEl = canvasRef.value
  if(canvasEl){
    ctx = canvasEl.getContext('2d')
  }
})
async function start(){
  if(!videoEl || !canvasEl || !ctx){
    alert("DOM没有加载完成，请刷新页面重试！")
    return
  }
  if(poseLandmarker) return;
  if(!vision){
    vision = await FilesetResolver.forVisionTasks(
      "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.15/wasm"
    );
  }
  poseLandmarker = await PoseLandmarker.createFromOptions(vision, {
    baseOptions: {
      // ========== 这里改了！使用public下本地模型 ==========
      modelAssetPath: "/pose_landmarker_full.task"
    },
    runningMode: "VIDEO",
    numPoses:1
  })
  stream = await navigator.mediaDevices.getUserMedia({video:true})
  videoEl.srcObject = stream
  await videoEl.play()
  canvasEl.width = videoEl.videoWidth
  canvasEl.height = videoEl.videoHeight
  loopDetect()
}
function loopDetect(){
  if(!poseLandmarker || !videoEl) return
  const res = poseLandmarker.detectForVideo(videoEl, performance.now())
  ctx.clearRect(0,0,canvasEl.width,canvasEl.height)
  ctx.drawImage(videoEl,0,0)
  if(res.landmarks.length>0){
    const lm = res.landmarks[0]
    ctx.strokeStyle="#00ff00"
    ctx.lineWidth=2
    POSE_CONNECTIONS.forEach(([s,e])=>{
      const p1 = lm[s]
      const p2 = lm[e]
      ctx.beginPath()
      ctx.moveTo(p1.x*canvasEl.width, p1.y*canvasEl.height)
      ctx.lineTo(p2.x*canvasEl.width, p2.y*canvasEl.height)
      ctx.stroke()
    })
    for(let p of lm){
      ctx.beginPath()
      ctx.arc(p.x*canvasEl.width, p.y*canvasEl.height,4,0,2*Math.PI)
      ctx.fillStyle="#ff0000"
      ctx.fill()
    }
  }
  animationId = requestAnimationFrame(loopDetect)
}
function stop(){
  if(animationId){
    cancelAnimationFrame(animationId)
    animationId = null
  }
  if(stream){
    stream.getTracks().forEach(t=>t.stop())
    stream = null
  }
  poseLandmarker = null
}
</script>
