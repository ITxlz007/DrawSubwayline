<template>
  <div class="panel-container">
    <!-- <div class="tooltip">
      <div class="top"></div>
      <div class="bottom"></div>
    </div> -->
    <div class="map" ref='map' style="height:100vh;width:100%;"></div>
    <div class="tip">
      <Tip-dialog ref="tipdialog" :message='message'></Tip-dialog>
    </div>
    <div class="legend">
      <div v-for="(item , index) in legendList" :key="index" class="bj">

        <span></span>
        <span>{{item.name}}</span>
      </div>
    </div>
  </div>
</template>

<script>
// 接口
import westStation from "@/service/api/weststation";
// 导入字典
import { BjwestStation } from "@/utils/dictionary";
//
import chartUtils from "@/utils/chartUtils";
// import { delete } from "vue/types/umd";
// import { delete } from "vue/types/umd";
const { echartsBar, bothwayBar, hsingleBar, doublePie } = chartUtils;
import CommonFun from "@/utils/commonFun";
// formatNumberToThousands
import tipdialog from "../stationMap/coms/tip-cotainer";
//  CommonFun.formatNumberToThousands(count),
export default {
  components: {
    "Tip-dialog": tipdialog,
  },
  data() {
    return {
      screenWidth: document.body.clientWidth,
      message: {
        title: "",
        subwayioData: {},
      },
      legendList: [
        { name: "2号线", color: "#009DFF" },
        { name: "4号线", color: "#1a9aa6" },
        { name: "7号线", color: "#FF9000" },
        { name: "9号线", color: "#AEFF15" },
        { name: "13号线", color: "#FFE500" },
        { name: "14号线", color: "#F4695B" },
      ],
    };
  },
  methods: {
    async BjSubwayMap() {
      this.$echarts.init(this.$refs.map).dispose();
      var ditie = {
        type: "FeatureCollection",
        features: [
          {
            type: "Feature",
            geometry: {
              type: "Polygon",
              coordinates: [
                [
                  [0, 0],
                  [0, 2118],
                  [2646, 2118],
                  [2646, 0],
                ],
              ],
            },
            properties: {
              name: "地铁图",
            },
          },
        ],
      };

      var data = [
        /*  // 地铁9号线l
        {
          name: "地铁9号线",
          tooltip: {
            formatter: "{b}: 19999<br />",
          },
          symbolSize: 0.1,
          value: [5, 100],
          x: 800,
          y: 400,
          fixed: true,
          // draggable: false,
          category: 1,
          label: {
            color: "#AEFF15",
            position: "bottom",
            fontSize: 24,
            fontWeight: 1000,
          },
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#157eff",
                },
                {
                  offset: 1,
                  color: "#35c2ff",
                },
              ]),
            },
          },
        },
        // 地铁14号线l
        {
          name: "地铁14号线",
          tooltip: {
            formatter: "{b}: 19999<br />",
          },
          symbolSize: 0.1,
          value: [920, 1050],
          x: 800,
          y: 400,
          fixed: true,
          // draggable: false,
          category: 1,
          label: {
            color: "#F4695B",
            position: "bottom",
            fontSize: 24,
            fontWeight: 1000,
          },
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#157eff",
                },
                {
                  offset: 1,
                  color: "#35c2ff",
                },
              ]),
            },
          },
        },
        // 地铁4号线l
        {
          name: "地铁4号线",
          tooltip: {
            formatter: "{b}: 19999<br />",
          },
          symbolSize: 0.1,
          value: [450, 100],
          x: 800,
          y: 400,
          fixed: true,
          // draggable: false,
          category: 1,
          label: {
            color: "#1a9aa6",
            position: "bottom",
            fontSize: 24,
            fontWeight: 1000,
          },
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#157eff",
                },
                {
                  offset: 1,
                  color: "#35c2ff",
                },
              ]),
            },
          },
        },
        // 地铁7号线l
        {
          name: "地铁7号线",
          tooltip: {
            formatter: "{b}: 19999<br />",
          },
          symbolSize: 0.1,
          value: [1040, 100],
          x: 800,
          y: 400,
          fixed: true,
          // draggable: false,
          category: 1,
          label: {
            color: "#FF9000",
            position: "bottom",
            fontSize: 24,
            fontWeight: 1000,
          },
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#157eff",
                },
                {
                  offset: 1,
                  color: "#35c2ff",
                },
              ]),
            },
          },
        },
        // 地铁2号线l
        {
          name: "地铁2号线",
          tooltip: {
            formatter: "{b}: 19999<br />",
          },
          symbolSize: 0.1,
          value: [300, 650],
          x: 800,
          y: 400,
          fixed: true,
          // draggable: false,
          category: 1,
          label: {
            color: "#009DFF",
            position: "bottom",
            fontSize: 24,
            fontWeight: 1000,
          },
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#157eff",
                },
                {
                  offset: 1,
                  color: "#35c2ff",
                },
              ]),
            },
          },
        },
        // 地铁13号线l
        {
          name: "地铁13号线",
          tooltip: {
            formatter: "{b}: 19999<br />",
          },
          symbolSize: 0.1,
          value: [500, 1050],
          x: 800,
          y: 400,
          fixed: true,
          // draggable: false,
          category: 1,
          label: {
            color: "#FFE500",
            position: "bottom",
            fontSize: 24,
            fontWeight: 1000,
          },
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#157eff",
                },
                {
                  offset: 1,
                  color: "#35c2ff",
                },
              ]),
            },
          },
        }, */
        // 地铁13号线
        {
          name: "大钟寺",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [310, 835],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FFE500",
                },
                {
                  offset: 1,
                  color: "#FFE500",
                },
              ]),
            },
          },
        },
        {
          name: "上地",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [310, 950],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FFE500",
                },
                {
                  offset: 1,
                  color: "#FFE500",
                },
              ]),
            },
          },
        },
        {
          name: "清河站",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
            backgroundColor: "#009dff",
            borderRadius: 5,
            padding: 5,
            fontSize: 16,
            fontWeight: "bold",
          },
          value: [310, 1000],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FFE500",
                },
                {
                  offset: 1,
                  color: "#FFE500",
                },
              ]),
            },
          },
        },
        {
          name: "西二旗",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [310, 1080],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FFE500",
                },
                {
                  offset: 1,
                  color: "#FFE500",
                },
              ]),
            },
          },
        },
        {
          name: "立水桥",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "top",
          },
          value: [800, 1080],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FFE500",
                },
                {
                  offset: 1,
                  color: "#FFE500",
                },
              ]),
            },
          },
        },
        {
          name: "柳芳",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [800, 835],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FFE500",
                },
                {
                  offset: 1,
                  color: "#FFE500",
                },
              ]),
            },
          },
        },
        //地铁2号线
        {
          name: "积水潭",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [410, 770],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "雍和宫",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "top",
          },
          value: [730, 770],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "东直门",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [760, 740],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "建国门",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [760, 500],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "磁器口",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "bottom",
          },
          value: [680, 450],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "北京站",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
            // textShadowColor: "#009dff",
            backgroundColor: "#009dff",
            borderRadius: 5,
            padding: 5,
            fontSize: 16,
            fontWeight: "bold",
          },
          value: [730, 450],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "长椿街",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [410, 450],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        {
          name: "复兴门",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [380, 500],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#009DFF",
                },
                {
                  offset: 1,
                  color: "#009DFF",
                },
              ]),
            },
          },
        },
        // 地铁9号线

        {
          name: "军事博物馆",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [200, 550],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#AEFF15",
                },
                {
                  offset: 1,
                  color: "#AEFF15",
                },
              ]),
            },
          },
        },

        {
          name: "六里桥东",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [170, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#AEFF15",
                },
                {
                  offset: 1,
                  color: "#AEFF15",
                },
              ]),
            },
          },
        },
        {
          name: "六里桥",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [120, 350],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#AEFF15",
                },
                {
                  offset: 1,
                  color: "#AEFF15",
                },
              ]),
            },
          },
        },
        {
          name: "七里庄",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [80, 300],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#AEFF15",
                },
                {
                  offset: 1,
                  color: "#AEFF15",
                },
              ]),
            },
          },
        },
        {
          name: "郭公庄",
          symbol: "circle",
          symbolSize: [20, 20],
          label: {
            color: "#efefef",
            position: "left",
          },
          value: [80, 100],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 1, 1, 0, [
                {
                  offset: 0,
                  color: "#AEFF15",
                },
                {
                  offset: 1,
                  color: "#AEFF15",
                },
              ]),
            },
          },
        },
        //地铁7号线
        {
          name: "北京西站",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
            backgroundColor: "#009dff",
            borderRadius: 5,
            padding: 5,
            fontSize: 16,
            fontWeight: "bold",
          },
          value: [200, 500],
          x: 1000,
          y: 1000,
          // fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "湾子",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "bottom",
          },
          value: [300, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },
        {
          name: "广安门内",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "bottom",
          },
          value: [450, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },
        {
          name: "虎坊桥",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "bottom",
          },
          value: [550, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },

        {
          name: "大郊亭",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "top",
          },
          value: [950, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },
        {
          name: "百子湾",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [1000, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },
        {
          name: "欢乐谷景区",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [1000, 250],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },
        {
          name: "花庄",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [1100, 100],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#FF9000",
                },
                {
                  offset: 1,
                  color: "#FF9000",
                },
              ]),
            },
          },
        },

        {
          name: "北京北站",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
            backgroundColor: "#009dff",
            borderRadius: 5,
            padding: 5,
            fontSize: 16,
            fontWeight: "bold",
          },
          value: [380, 730],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        // 地铁4号线
        {
          name: "安河桥北",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "white",
            position: "bottom",
          },
          value: [50, 1050],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "圆明园",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "white",
            position: "right",
          },
          value: [200, 1050],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "国家图书馆",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "left",
          },
          value: [200, 700],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "动物园",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "bottom",
          },
          value: [330, 700],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "西直门",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [380, 700],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "新街口",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [500, 700],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "宣武门",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [500, 450],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "菜市口",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [500, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "陶然亭",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [500, 370],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "马家堡",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [500, 300],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        {
          name: "天宫院",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#fff",
            position: "right",
          },
          value: [500, 20],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#1a9aa6",
                },
                {
                  offset: 1,
                  color: "#1a9aa6",
                },
              ]),
            },
          },
        },
        //地铁14号线
        {
          name: "北京南站",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "left",
            backgroundColor: "#009dff",
            borderRadius: 5,
            padding: 5,
            fontSize: 16,
            fontWeight: "bold",
          },
          value: [500, 350],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "永定门外",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "bottom",
          },
          value: [600, 350],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "方庄",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [780, 350],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "十里河",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "bottom",
          },
          value: [850, 300],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "北工大西门",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [920, 300],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "九龙山",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [920, 400],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "大望路",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [920, 500],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "将台",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [920, 700],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
        {
          name: "望京南",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [850, 800],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },

        {
          name: "善各庄",
          symbol: "circle",
          symbolSize: [10, 10],
          label: {
            color: "#efefef",
            position: "right",
          },
          value: [850, 1080],
          x: 1000,
          y: 1000,
          fixed: true,
          category: 2,
          itemStyle: {
            normal: {
              color: new this.$echarts.graphic.LinearGradient(0, 0, 1, 0, [
                {
                  offset: 0,
                  color: "#F4695B",
                },
                {
                  offset: 1,
                  color: "#F4695B",
                },
              ]),
            },
          },
        },
      ];

      var option = {
        title: {
          text: "北京铁路站区地铁线路",
          textStyle: {
            color: "white",
            fontSize: 20,
          },
          x: "center",
          top: 10,
        },
        //不设置背景颜色就是透明色
        // backgroundColor: "#0d215d",
        /* legend: {
          // show: true,
          // type: "plain",
          // left: "20px",
          // data: ["2号线", "4号线", "7号线", "9号线", "13号线", "14号线"],
          data: ["2号线", "4号线", "7号线", "9号线", "13号线", "14号线"],
        }, */
        xAxis: {
          show: false,
          min: 0,
          max: 1200,
          // type: "value",
          //开启x轴坐标
          axisPointer: {
            show: true,
          },
        },
        /* geo: {
          map: "ditie",
          width: "95%",
          height: "95%",
          roam: true,
          itemStyle: {
            color: "transparent",
            borderWidth: 0,
          },
          label: {
            show: false,
          },
          emphasis: {
            itemStyle: {
              color: "transparent",
              borderWidth: 0,
            },
            label: {
              show: false,
            },
          },
        }, */
        yAxis: {
          show: false,
          min: 0,
          max: 1200,
          //   type: "value",
          //开启y轴坐标
          axisPointer: {
            show: true,
          },
        },
        axisPointer: {
          show: false,
        },
        tooltip: {
          show: true,
          trigger: "item",
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: "cross", // 默认为直线，可选为：'line' | 'shadow'
          },
          // mousemove|click
          // 隐藏坐标线和弹窗 以及鼠标放上去的提示框
          triggerOn: "none",
        },
        //  legend: {
        //     show: false
        //  },
        series: [
          {
            type: "graph",
            zlevel: 5,
            draggable: false,
            roam: true,
            coordinateSystem: "cartesian2d", //使用二维的直角坐标系（也称笛卡尔坐标系）

            // edgeSymbolSize: [0, 8], //边两端的标记大小，可以是一个数组分别指定两端，也可以是单个统一指定
            // edgeLabel: {
            //   normal: {
            //     textStyle: {
            //       fontSize: 60
            //     }
            //   }
            // },
            symbol: "rect",
            symbolOffset: ["15%", 0],
            // markPoint: {
            //   symbol: "diamond",
            // },
            label: {
              normal: {
                show: true,
              },
            },
            data: data,
            links: [
              //地铁13号线
              {
                source: "大钟寺",
                target: "清河站",
                lineStyle: {
                  normal: {
                    color: "#FFE500",
                  },
                },
              },
              {
                source: "清河站",
                target: "西二旗",
                lineStyle: {
                  normal: {
                    color: "#FFE500",
                  },
                },
              },
              {
                source: "西二旗",
                target: "立水桥",
                lineStyle: {
                  normal: {
                    color: "#FFE500",
                  },
                },
              },
              {
                source: "立水桥",
                target: "柳芳",
                lineStyle: {
                  normal: {
                    color: "#FFE500",
                  },
                },
              },

              //地铁二号线连接
              {
                source: "西直门",
                target: "积水潭",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "积水潭",
                target: "雍和宫",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "柳芳",
                target: "东直门",
                lineStyle: {
                  normal: {
                    color: "#FFE500",
                  },
                },
              },
              {
                source: "雍和宫",
                target: "东直门",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "东直门",
                target: "建国门",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "建国门",
                target: "北京站",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },

              {
                source: "北京站",
                target: "磁器口",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "磁器口",
                target: "宣武门",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "宣武门",
                target: "长椿街",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "长椿街",
                target: "复兴门",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              {
                source: "复兴门",
                target: "西直门",
                lineStyle: {
                  normal: {
                    // color: "red",
                  },
                },
              },
              // 地铁9号线

              {
                source: "军事博物馆",
                target: "北京西站",
                lineStyle: {
                  normal: {
                    color: "#AEFF15",
                  },
                },
              },
              {
                source: "湾子",
                target: "北京西站",
                lineStyle: {
                  normal: {
                    color: "#FF9000",
                  },
                },
              },
              {
                source: "北京西站",
                target: "六里桥东",
                lineStyle: {
                  normal: {
                    color: "#AEFF15",
                  },
                },
              },
              {
                source: "六里桥东",
                target: "六里桥",
                lineStyle: {
                  normal: {
                    color: "#AEFF15",
                  },
                },
              },
              {
                source: "六里桥",
                target: "七里庄",
                lineStyle: {
                  normal: {
                    color: "#AEFF15",
                  },
                },
              },
              {
                source: "七里庄",
                target: "郭公庄",
                lineStyle: {
                  normal: {
                    color: "#AEFF15",
                  },
                },
              },
              // 地铁7号线

              {
                source: "湾子",
                target: "百子湾",
                lineStyle: {
                  normal: {
                    color: "#FF9000",
                  },
                },
              },
              {
                source: "百子湾",
                target: "欢乐谷景区",
                lineStyle: {
                  normal: {
                    color: "#FF9000",
                  },
                },
              },
              {
                source: "欢乐谷景区",
                target: "花庄",
                lineStyle: {
                  normal: {
                    color: "##FF9000",
                  },
                },
              },

              // 北京地铁4号线地铁绘制
              {
                source: "安河桥北",
                target: "圆明园",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },

              {
                source: "军事博物馆",
                target: "国家图书馆",
                lineStyle: {
                  normal: {
                    color: "#AEFF15",
                  },
                },
              },
              {
                source: "圆明园",
                target: "国家图书馆",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "国家图书馆",
                target: "动物园",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "大钟寺",
                target: "西直门",
                lineStyle: {
                  normal: {
                    color: "#FFE500",
                  },
                },
              },
              {
                source: "动物园",
                target: "西直门",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "西直门",
                target: "新街口",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "新街口",
                target: "陶然亭",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "陶然亭",
                target: "北京南站",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "北京南站",
                target: "马家堡",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              {
                source: "马家堡",
                target: "天宫院",
                lineStyle: {
                  normal: {
                    color: "#1a9aa6",
                  },
                },
              },
              // 北京地铁14号线线路
              {
                source: "北京南站",
                target: "永定门外",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "永定门外",
                target: "方庄",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "方庄",
                target: "十里河",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "十里河",
                target: "北工大西门",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "北工大西门",
                target: "九龙山",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "九龙山",
                target: "大望路",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "大望路",
                target: "将台",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
              {
                source: "将台",
                target: "望京南",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },

              {
                source: "望京南",
                target: "善各庄",
                lineStyle: {
                  normal: {
                    color: "#F4695B",
                  },
                },
              },
            ],
            lineStyle: {
              normal: {
                opacity: 0.6, //线条透明度
                color: "#53B5EA",
                curveness: 0, //站点间连线曲度，0表示直线
                width: 3, //线条宽度
              },
            },
          },
          {
            // 散点图用来标记5个站区
            type: "effectScatter",
            scaling: 1.5,
            color: "#00d2ff",
            // roam: true,
            // 设置层级让散点悬浮在节点上方便于单击事件
            zlevel: 7,
            //该系列使用的坐标系
            coordinateSystem: "cartesian2d",
            symbolSize: function (val) {
              return val[2] * 0.8;
            },
            data: [
              { name: "北京站", value: [730, 450, 10.89] },
              { name: "北京西站", value: [200, 500, 10.89] },
              { name: "北京南站", value: [500, 350, 10.89] },
              { name: "北京北站", value: [380, 730, 10.89] },
              { name: "清河站", value: [310, 1000, 10.89] },
            ],
            showEffectOn: "render",
            effectType: "ripple",
            rippleEffect: {
              period: 4,
              scale: 4,
              brushType: "stroke",
            },
            hoverAnimation: true,
          },
          {
            type: "lines",
            coordinateSystem: "cartesian2d",
            z: 2,
            zlevel: 7,
            animation: true,
            effect: {
              show: true,
              period: 8,
              trailLength: 0.71,
              // symbolSize: 14,
              symbolSize: 10,
              symbol: "circle",
              loop: true,
              color: "",
              //   color: "rgba(55,155,255,0.5)"
            },
            lineStyle: {
              normal: {
                color: "green",
                width: 0,
                opacity: 1, //尾迹线条透明度
                curveness: 0, //动画线路的曲度
              },
            },
            //设置这个属性用于可以沿着曲线路径运动
            polyline: true,
            data: [
              {
                name: "13号线",
                lineStyle: {
                  width: 3,
                  color: "#FFE500",
                },
                // 13号线
                coords: [
                  [760, 740],
                  [800, 835],
                  [800, 1080],
                  [310, 1080],
                  [310, 835],
                  [380, 700],
                  // [750,1080],
                  // [],
                  // [],
                ],
              },
              {
                name: "2号线",
                lineStyle: {
                  width: 3,
                  color: "#009DFF",
                },
                // 2号线
                coords: [
                  [380, 500],
                  [410, 450],

                  [730, 450],
                  [760, 500],

                  [760, 740],
                  [730, 770],
                  [410, 770],
                  [380, 700],
                  [380, 500],
                  // [],
                ],
              },
              {
                name: "7号线",
                lineStyle: {
                  width: 3,
                  color: "#FF9000",
                },
                //7号线
                coords: [
                  [200, 500],
                  [300, 400],
                  [1000, 400],
                  [1000, 250],
                  // g
                  [1100, 100],
                  //  [200,700],
                ].reverse(),
              },
              {
                //9号线
                name: "9号线",
                lineStyle: {
                  width: 3,
                  color: "#AEFF15",
                },
                coords: [
                  [80, 100],
                  [80, 300],
                  [120, 350],
                  [170, 400],
                  // g
                  [200, 500],
                  [200, 700],
                ],
              },
              {
                name: "4号线",
                lineStyle: {
                  width: 3,
                  color: "#00E5FF",
                },
                //4号线
                coords: [
                  [500, 20],
                  [500, 700],
                  // g
                  [200, 700],
                  [200, 1050],
                  [50, 1050],
                ],
              },
              {
                //14号线
                name: "14号线",
                lineStyle: {
                  width: 3,
                  color: "#F4695B",
                },
                coords: [
                  [500, 350],
                  [780, 350],
                  [850, 300],
                  [920, 300],
                  [920, 700],
                  [850, 800],
                  [850, 1080],
                ].reverse(),
              },
            ],
          },
        ],
      };

      //   var mycharts  = this.$this.$echarts.init()
      var mycharts = this.$echarts.init(
        this.$refs.map || document.getElementById("map")
      );
      this.$echarts.registerMap("ditie", ditie);
      mycharts.setOption(option);
      //  mycharts.getZr().on
      // document.on("click", function (params) {
      //   console.log("paramsparams", params);
      // });
      // this = that;
      // 如果是分站的话散点图就取消单击事件
      if (this.hasAllPermission) {
        let that = this;
        mycharts.on("click", function (params) {
          /* console.log(
          "param",
          params,
          params.event.event.zrX,
          params.event.event.zrY
        ); */

          if (params.componentSubType === "effectScatter") {
            // 进行操作
            that.$nextTick(() => {
              // 开启弹窗
              that.message.title = params.name;
              that.getCenterData(params.name);

              that.$refs.tipdialog.dialogVisible = true;
              // this.getMousePos();
              // console.log(" this.getMousePos()", this.getMousePos());
            });
            // todo...
            console.log(
              "componentSubType",
              params.componentSubType,
              params.name
            );
            // this.$message.info(params.name);
            // that.$message({
            //   type: "success",
            //   message: params.name,
            // });
          }

          // }
        });
      }

      // var ecConfig = this.$echarts.config;
      // var ecConfig = require("echarts/config");
      // mycharts.getZr().on(ecConfig.event.click, this.eConsole(val));
    },
    getMousePos(event) {
      var e = event || window.event;
      var scrollX =
        document.documentElement.scrollLeft || document.body.scrollLeft;
      var scrollY =
        document.documentElement.scrollTop || document.body.scrollTop;
      var x = e.pageX || e.clientX + scrollX;
      var y = e.pageY || e.clientY + scrollY;
      //alert('x: ' + x + '\ny: ' + y);
      return { x: x, y: y };
    },

    eConsole(param) {
      console.log("param", param);
      // }
    },
    async getCenterData(stationName) {
      let res = await westStation.getCenterData({ stationName: stationName });
      let { todayList, yesterdayList } = res;
      todayList = todayList.slice(-2).map((item) => {
        return {
          head: this.getInStation(item.head, "进站", "出站"),

          count: CommonFun.formatNumberToThousands(item.count),
        };
      });
      yesterdayList = yesterdayList.slice(-2).map((item) => {
        return {
          head: this.getInStation(item.head, "进站", "出站"),
          count: CommonFun.formatNumberToThousands(item.count),
        };
      });
      this.message.subwayioData = {
        tdList: [...todayList],
        ydList: [...yesterdayList],
      };
      console.log(stationName, this.message.subwayioData);
    },
    getInStation(str, currentStr1, currentStr2) {
      if (str.indexOf(currentStr1) != -1) {
        return currentStr1;
      } else if (str.indexOf(currentStr2) != -1) {
        return currentStr2;
      }
    },
  },
  computed: {
    hasAllPermission() {
      return CommonFun.hasAllPermission();
    },
  },
  created() {},
  mounted() {
    this.BjSubwayMap();
    const that = this;
    window.onresize = () => {
      return (() => {
        window.screenWidth = document.body.clientWidth;
        that.screenWidth = window.screenWidth;
      })();
    };
  },
  watch: {
    screenWidth(nval, oval) {
      console.log("窗口", nval, oval);
      if (nval != oval) {
        // debugger
        this.BjSubwayMap();
      }
    },
  },
};
</script>

<style lang="less" scoped>
.panel-container {
  width: 100%;
  height: 100%;
  position: relative;
  .map {
    height: 100%;
    width: 100%;
  }
  .tooltip {
    height: 100px;
    width: 400px;
    background-color: #0b1b58;
    position: absolute;
    left: 20px;
    top: 20px;
    z-index: 100;
  }
  .tip {
    position: absolute;
    left: 0;
    top: 50px;
    // right: 0;
  }
  .legend {
    position: absolute;
    left: 10px;
    top: 10px;
    color: white;
    .bj {
      span:nth-of-type(1) {
        display: inline-block;
        height: 10px;
        width: 20px;
        margin-right: 10px;
      }
      &:nth-of-type(1) {
        span:nth-of-type(1) {
          background-color: #009dff;
        }
      }
      &:nth-of-type(2) {
        span:nth-of-type(1) {
          background-color: #1a9aa6;
        }
      }
      &:nth-of-type(3) {
        span:nth-of-type(1) {
          background-color: #ff9000;
        }
      }
      &:nth-of-type(4) {
        span:nth-of-type(1) {
          background-color: #aeff15;
        }
      }
      &:nth-of-type(5) {
        span:nth-of-type(1) {
          background-color: #ffe500;
        }
      }
      &:nth-of-type(6) {
        span:nth-of-type(1) {
          background-color: #f4695b;
        }
      }
      &:nth-of-type(7) {
        span:nth-of-type(1) {
          background-color: #009dff;
        }
      }
    }
  }
}
</style>
