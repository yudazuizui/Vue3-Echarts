<template>
	<div class="container">
		<div class="screen" ref="screen">
		<div
		class=" h-screen text-white p-2 flex overflow-hidden"
		v-if="data"
	>
		<div class="flex-1 mr-2 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
			<!-- 横向柱状图 -->
			<HorizontalBar class="h-1/3 box-border pb-4" :data="data.regionData" />
			<!-- 雷达图 -->
			<RadarBar class="h-1/3 box-border pb-4" :data="data.riskData" />
			<!-- 数据传递关系图 -->
			<Relation class="h-1/3" :data="data.relationData" />
		</div>
		<div class="w-1/2 mr-2 flex flex-col">
			<!-- 数据展示图 -->
			<TotalData
				class="bg-opacity-50 bg-slate-800 p-3"
				:data="data.totalData"
			/>
			<!-- 地图可视化 -->
			<MapChart
				class="bg-opacity-50 bg-slate-800 p-3 mt-2 flex-1"
				:data="data.mapData"
			/>
		</div>
		<div class="flex-1 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
			<!-- 竖向柱状图 -->
			<VerticalBar class="h-1/3 box-border pb-4" :data="data.serverData" />
			<!-- 环形资源站比图 -->
			<RadiueBar class="h-1/3 box-border pb-4" :data="data.abnormalData" />
			<!-- 文档云图 -->
			<WordCloud class="h-1/3 box-border" :data="data.wordCloudData" />
		</div>
	</div>
	</div>
	</div>
</template>

<script setup>
import HorizontalBar from './components/HorizontalBar.vue'
import RadarBar from './components/RadarBar.vue'
import Relation from './components/Relation.vue'
import TotalData from './components/TotalData.vue'
import MapChart from './components/MapChart.vue'
import VerticalBar from './components/VerticalBar.vue'
import RadiueBar from './components/RadiueBar.vue'
import WordCloud from './components/WordCloud.vue'

import { ref,onMounted } from 'vue'
import { getVisualization } from '@/api/visualization.js'
const screen = ref('')
const data = ref(null)

const loadData = async () => {
	data.value = await getVisualization()
	// console.log(data.value)
}
onMounted(()=>{
	console.log(11)
	screen.value.style.transform=`scale(${getRize()})translate(-50%,-50%)`
})
window.onresize= ()=>{
	console.log(22)
	screen.value.style.transform=`scale(${getRize()})translate(-50%,-50%)`
}
loadData()
function getRize(x=1920,y=1080){
	const dd = window.innerWidth/x
	const tt = window.innerHeight/y
	return dd<tt ?dd:tt
}
setInterval(() => {
	loadData()
}, 3000)
</script>
<style scoped>
.container {
	width: 100vw;
	height: 100vh;
	background: url('./assets/imgs/bg.jpg') no-repeat;
	background-size: cover;
}
.screen{
	width: 1920px;
	height: 1080px;
	transform-origin: left top;
	position: fixed;
	top: 50%;
	left: 50%;
}
</style>
