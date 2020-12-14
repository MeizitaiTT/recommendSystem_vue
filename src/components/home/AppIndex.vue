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

        <a-card id="introduction" title="签到数据分布" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          <div id="container" style="width:600px;height:500px;"></div>
        </a-card>
        <br>

        <a-card id="technology" title="技术" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          技术
        </a-card>
        <br>

        <a-card id="algorithm" title="算法" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          算法
        </a-card>
        <br>

      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script>
import SideMenu from '@/components/common/SideMenu'
import { TMap } from '../../TMap'
export default {
  components: { SideMenu },
  data () {
    return {
      sideMenuList: [
        { href: '#introduction', name: '介绍', type: 'user' },
        { href: '#technology', name: '技术', type: 'laptop' },
        { href: '#algorithm', name: '算法', type: 'notification' }
      ],
      selectedKeys: [],
      business: []
    }
  },
  mounted () {
    this.$axios.get('/business/longitudeLatitude').then(res => {
      console.log(res.data)
      this.business = res.data.data.businessDTOList
    })
    TMap('2ZQBZ-O4NLI-BG5GK-5Y6FL-C3KW2-ZRBLS').then(qq => {
      var map = new qq.maps.Map(document.getElementById('container'), {
        // 地图的中心地理坐标。
        center: new qq.maps.LatLng(36.421643, -100.892332),
        zoom: 3
      })
      for (let i = 0; i < this.business.length; i++) {
        var Latlng = new qq.maps.LatLng(this.business[i].latitude, this.business[i].longitude)
        var marker = new qq.maps.Marker({
          position: Latlng,
          map: map,
          content: '1'
        })
      }
    })
  },
  methods: {
  }
}
</script>

<style scoped>
</style>
