<template>
  <div>
    <div id="main" style="width: 1000x; height: 400px"></div>
    <button @click="reset">重置</button>
  </div>
</template>

<script setup>
import * as echarts from "echarts";
import { ref, onMounted, computed } from "vue";
const seriesData = ref([
  {
    data: [
      0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
      0,
    ],
    type: "line",
  },
  {
    data: [],
    type: "line",
  },
]);
const chartOption = computed(() => ({
  xAxis: {
    type: "category",
    data: [
      "A",
      "B",
      "C",
      "D",
      "E",
      "F",
      "G",
      "H",
      "I",
      "J",
      "K",
      "L",
      "M",
      "N",
      "O",
      "P",
      "Q",
      "R",
      "S",
      "T",
      "U",
      "V",
      "W",
      "X",
      "Y",
      "Z",
    ],
  },
  tooltip: {
    trigger: "axis",
    axisPointer: {
      type: "cross",
      label: {
        backgroundColor: "#6a7985",
      },
    },
  },
  yAxis: {
    type: "value",
  },
  series: seriesData.value,
}));

const initEcharts = () => {
  const handleCoord = (xIndex, yIndex) => {
    const pushData = (xIndex) => {
      seriesData.value[1].data.push(xIndex);
      myChart.setOption(chartOption.value);
    };
    if (seriesData.value[1].data.length == yIndex) pushData(xIndex);
  };
  var myChart = echarts.init(document.getElementById("main"));
  myChart.setOption(chartOption.value);
  myChart.off("showTip");
  myChart.on("showTip", function (params) {
    const result = this.convertFromPixel(
      {
        seriesIndex: 0,
        xAxisIndex: 0,
      },
      [params.x, params.y]
    );
    handleCoord(result[1], result[0]);
  });
};

const reset = () => {
  seriesData.value[1].data = [];
  initEcharts();
};

onMounted(() => {
  initEcharts();
});
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
