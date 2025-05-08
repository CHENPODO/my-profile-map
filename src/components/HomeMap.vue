<script setup>
import mapImage from "@/assets/HomeMap.png"
import CatImage from "@/assets/logo.png"

import { ref, onMounted, onUnmounted } from "vue"
// 監聽鍵盤
const handleKeydown = (e) => {
	// console.log("你按下的鍵是：", e.key)
	switch (e.key) {
		case "w":
		case "W":
			posY.value -= step
			break
		case "s":
		case "S":
			posY.value += step
			break
		case "a":
		case "A":
			posX.value -= step
			break
		case "d":
		case "D":
			posX.value += step
			break
	}
}

// 離開元件時記得清除事件監聽
onUnmounted(() => {
	window.removeEventListener("keydown", handleKeydown)
})
// 定義貓咪移動的位置
const posX = ref(0)
const posY = ref(0)
const step = 10 // 貓咪移動的步伐

// 掛載時，生成貓咪的初始位置
onMounted(() => {
	const screenWidth = window.innerWidth
	const screenHeight = window.innerHeight
	const catSize = 64 //對應 w-12 h-12
	// 當鍵盤被按下時，自動執行 handleKeydown，並傳入 e
	window.addEventListener("keydown", handleKeydown)

	// 水平
	posX.value = screenWidth / 2 - catSize / 2 //正中央
	// 垂直底部往上 40px
	posY.value = screenHeight - catSize - 40 // 離底部一點點上方
})
</script>
<template>
	<!-- 地圖背景 -->
	<div class="w-screen h-screen relative overflow-hidden">
		<img class="map-bg" :src="mapImage" alt="web-homepage" />
		<!-- 移動貓咪 -->
		<img class="absolute w-16 h-16" :src="CatImage" :style="{ top: `${posY}px`, left: `${posX}px` }" alt="CatImage" />
	</div>
</template>
<style>
.map-bg {
	width: 100%;
	height: 100%;
	object-fit: cover;
}
</style>
