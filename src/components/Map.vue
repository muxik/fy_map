<template>
  <div class="map">
    <div class="container">
      <div id="mapbox" ref="mapbox"></div>
    </div>
  </div>
</template>

<script>
import echarts from "echarts";
import jsonp from "jsonp";
// China Map
import "echarts/map/js/china";
import "bootstrap/dist/css/bootstrap.min.css";

const option = {
  title: {
    text: "Muxi_k Emap",
    link: "http://github.com/muxik",
    subtext: "念念不忘,必有回响",
    sublink: "http://github.com/muxik"
  },
  series: [
    {
      name: "确诊人数",
      // 渲染地图 <China>
      type: "map",
      map: "china",
      label: {
        show: true,
        fontSize: 9
      },
      itemStyle: {
        // 地图颜色 & 边框
        areaColor: "#fff",
        borderColor: "#000"
      },
      // 鼠标放大地图以及拖拽
      roam: false,
      // 地图宽高
      zoom: 0.9,
      emphasis: {
        // 鼠标划过
        label: {
          color: "#222",
          fontSize: 13
        }
      },
      data: []
    }
  ],
  visualMap: {
    type: "piecewise",
    show: true,
    pieces: [
      { min: 10000 },
      {
        min: 1000,
        max: 9999
      },
      {
        min: 100,
        max: 999
      },
      {
        min: 10,
        max: 99
      },
      {
        min: 1,
        max: 10
      },
      { min: 0, max: 0 }
    ],
    inRange: {
      symbol: "rect",
      color: ["rgb(231, 184, 184)", "#9c0505"]
    },
    itemWidth: 40,
    itemHeight: 19
  },
  tooltip: {
    trigger: "item"
  },
  toolbox: {
    show: true,
    orient: "vertical",
    left: "right",
    top: "center",
    feature: {
      dataView: { readOnly: true },
      restore: {},
      saveAsImage: {}
    }
  }
};

export default {
  name: "Map",
  mounted() {
    this.getData();
    this.mychart = echarts.init(this.$refs.mapbox);
    this.mychart.setOption(option);
  },
  methods: {
    getData() {
      jsonp(
        "http://interface.sina.cn/news/wap/fymap2020_data.d.json",
        {},
        (err, data) => {
          if (!err) {
            let list = data.data.list.map(item => ({
              name: item.name,
              value: item.value
            }));
            option.series[0].data = list;
            this.mychart.setOption(option);
          }
        }
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#mapbox {
  width: 1000px auto;
  height: 1000px;
}
</style>
