<script setup>
import { ref } from 'vue';

const data = new Array(500).fill(0).map((_, i) => i); // 模拟真实数据
const viewHeight = ref(400); // 可视容器高度
const itemHeight = ref(20); // 每一项的高度

const showData =ref([]) //要显示的数据
showData.value = data.slice(0,20) //先截取前20条数据

const scrollTop = ref(0) //初始滚动距离
//滚动事件触发函数
const handleScroll = (e) =>{
  //获取滚动的距离
  scrollTop.value = e.target.scrollTop
  //计算初始索引 ：滚动距离/每一项的高度
  const startIndex = Math.round(scrollTop.value / itemHeight.value)
  //结束索引：初始索引+容器高度/每一项高度
  const endIndex = startIndex + viewHeight.value / itemHeight.value
  //截取数据
  showData.value = data.slice(startIndex,endIndex)
}
</script>

<template>
  <div :style="{ height: viewHeight + 'px' }"  @scroll="handleScroll"  class="view-container">
    <div  :style="{
        height: itemHeight * data.length + 'px',
      }" class="content-container"></div>
    <div class="item-container"
    :style="{
          transform: 'translateY(' + scrollTop + 'px)',
        }"
    >
      <div  :style="{
          height: itemHeight + 'px',
        }" v-for="(item,index) in showData" class="item">{{ item }}</div>
    </div>
  </div>
</template>


<style scoped>
.view-container {
  height: 400px;
  width: 200px;
  border: 1px solid red;
  overflow-y: scroll;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.content-container {
  height: 1000px;
}

.item-container {
  position: absolute;
  top: 0;
  left: 0;
}

.item {
  height: 20px;
}

</style>
