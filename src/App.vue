<template>
  <div>
    <div id="echarts" ref="container"></div>
  </div>
</template>

<script>
import * as echarts from "echarts";
import "echarts-gl";
//这个json文件，通过跳转地址获取
import henanJson from "./assets/map/henan.json";
export default {
  data() {
    return {
      href: " http://datav.aliyun.com/tools/atlas/#&lat=33.521903996156105&lng=104.29849999999999&zoom=4", //这个网站注册其他省的地图
      option: {},
    };
  },
  methods: {
    init() {
      echarts.registerMap("henan", henanJson);
      this.option = {
        geo3D: {
          map: "henan",
          boxWidth: 100, //三维地理坐标系组件在三维场景中的宽度
          boxHeight: 10, //三维地理坐标系组件在三维场景中的高度
          boxDepth: "auto", //三维地理坐标系组件在三维场景中的深度
          regionHeight: 5, //地图片的高度
          // environthisnt: 'auto',
          //鼠标旋转，缩放等视觉设置
          viewControl: {
            // center: [-10, 0, 10]
          },
          //标签样式  即各个市的名字
          label: {
            show: true,
            formatter: function (params) {
              //回调函数返回各个省的县名称
              var content = "",
                content = params.name;
              return content;
            },
            distance: 0, //标签距离图形的距离，实测没啥用
            textStyle: {
              //实测改模块下都不管用
              color: "#DC143C",
              fontSize: 20,
              fontWeight: "bolder",
            },
          },
          //地图样式 ===》地图各省市区的颜色，边界
          itemStyle: {
            color: "#73a4ff",
            borderColor: "rgb(62,215,213)",
            borderWidth: 1,
          },
          //鼠标 hover 高亮时图形和标签的样式
          emphasis: {
            label: {
              show: true,
              distance: 10,
            },
            itemStyle: {
              color: "#ffa8cc",
            },
          },
          //地图区域的设置，要和下面series匹配很多东西。感觉是重点配置区域，想实现每个市颜色不一样，可能是在这儿配置。但是配置以后会出问题，和hover冲突
          // regions:[{
          //   name:'河南'
          // }],
          // shading :"realistic",//三维地理坐标系组件中三维图形的着色效果 。没啥用。
          // realisticMaterial:{},//真实感材质相关的配置项，在 shading 为'realistic'时有效。
          // colorMaterial:{}//color 材质相关的配置项，在 shading 为'color'时有效。

          //光源的设置
          light: {
            main: {
              intensity: 1,
              shadow: true,
              alpha: 150,
              beta: 70,
            },
            ambient: {
              intensity: 0,
            },
          },
          //地面的背景颜色  目前是透明
          groundPlane: {
            show: true,
            color: "transparent",
          },
          //特效设置
          postEffect: {
            enable: false,
          },


          series: [  //重点配置区域
            {
              type: 'bar3D',
              name:'河南',
              coordinateSystem: 'geo3D',
              
              itemStyle: {
                color: 'red',
                opacity: 1
              },
              emphasis:{
                label:{
                  show:true,
                }
              },
              // data: [],//重点中的重点
            }
          ]
        },
      };

      let initEcharts = echarts.init(this.$refs.container);
      initEcharts.setOption(this.option);
    },
  },
  created() {},
  mounted() {
    this.$nextTick(() => {
      this.init();
    });
  },
};
</script>

<style scoped>
#echarts {
  width: 100%;
  height: 700px;
}
</style>

