<template>
  <a-layout id="components-layout-demo-top-side-2">
    <a-layout>
      <a-layout-sider width="200" style="background: #fff">
        <side-menu :sideMenuList="sideMenuList" :selectedKeys="selectedKeys"></side-menu>
      </a-layout-sider>
      <a-layout style="padding: 0 24px 24px">
        <a-breadcrumb style="margin: 16px 0">
          <a-breadcrumb-item>首页</a-breadcrumb-item>
        </a-breadcrumb>

        <a-card id="recall" title="召回率" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          <div id="recall-graphic" style="width: 90%;height:400px;"></div>
        </a-card>
        <br>

        <a-card id="nDCG" title="nDCG" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          <div id="ndcg-graphic" style="width: 90%;height:400px;"></div>
        </a-card>
        <br>

        <a-card id="precision " title="精确度" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          算法
        </a-card>
        <br>

      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script>
import SideMenu from '@/components/common/SideMenu'
export default {
  components: { SideMenu },
  data () {
    return {
      sideMenuList: [
        { href: '#recall', name: '介绍', type: 'user' },
        { href: '#nDCG', name: '技术', type: 'laptop' },
        { href: '#precision', name: '算法', type: 'notification' }
      ],
      selectedKeys: []
    }
  },
  methods: {
    drawChart () {
      // 基于准备好的dom，初始化echarts实例
      let recallGraphic = this.$echarts.init(document.getElementById('recall-graphic'))
      let nDCG = this.$echarts.init(document.getElementById('ndcg-graphic'))
      // 指定图表的配置项和数据
      let option1 = {
        legend: {},
        tooltip: {},
        dataset: {
          dimensions: ['topic', 'TJM', 'SAETM', 'SPORE', 'USAETMM', 'STSTM'],
          source: [
            {topic: '20', 'TJM': 0.05, 'SAETM': 0.04, 'SPORE': 0.055, 'USAETMM': 0.05, 'STSTM': 0.068},
            {topic: '40', 'TJM': 0.06, 'SAETM': 0.045, 'SPORE': 0.086, 'USAETMM': 0.080, 'STSTM': 0.11},
            {topic: '60', 'TJM': 0.084, 'SAETM': 0.055, 'SPORE': 0.108, 'USAETMM': 0.089, 'STSTM': 0.151},
            {topic: '80', 'TJM': 0.091, 'SAETM': 0.06, 'SPORE': 0.115, 'USAETMM': 0.118, 'STSTM': 0.16},
            {topic: '120', 'TJM': 0.071, 'SAETM': 0.07, 'SPORE': 0.128, 'USAETMM': 0.088, 'STSTM': 0.141}
          ]
        },
        xAxis: {type: 'category'},
        yAxis: {},
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          {type: 'line'},
          {type: 'line'},
          {type: 'line'},
          {type: 'line'},
          {type: 'line'}
        ]
      }
      let option2 = {
        legend: {},
        tooltip: {},
        dataset: {
          dimensions: ['topic', 'TJM', 'SAETM', 'SPORE', 'USAETMM', 'STSTM'],
          source: [
            {topic: '20', 'TJM': 0.05, 'SAETM': 0.04, 'SPORE': 0.055, 'USAETMM': 0.05, 'STSTM': 0.068},
            {topic: '40', 'TJM': 0.06, 'SAETM': 0.045, 'SPORE': 0.086, 'USAETMM': 0.080, 'STSTM': 0.11},
            {topic: '60', 'TJM': 0.084, 'SAETM': 0.055, 'SPORE': 0.108, 'USAETMM': 0.089, 'STSTM': 0.151},
            {topic: '80', 'TJM': 0.091, 'SAETM': 0.06, 'SPORE': 0.115, 'USAETMM': 0.118, 'STSTM': 0.16},
            {topic: '120', 'TJM': 0.071, 'SAETM': 0.07, 'SPORE': 0.128, 'USAETMM': 0.088, 'STSTM': 0.141}
          ]
        },
        xAxis: {type: 'category'},
        yAxis: {},
        // Declare several bar series, each will be mapped
        // to a column of dataset.source by default.
        series: [
          {type: 'bar'},
          {type: 'bar'},
          {type: 'bar'},
          {type: 'bar'},
          {type: 'bar'}
        ]
      }
      // 使用刚指定的配置项和数据显示图表
      recallGraphic.setOption(option1)
      nDCG.setOption(option2)
    }
  },
  mounted () {
    this.drawChart()
  }
}
</script>

<style scoped>
</style>
