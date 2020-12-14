<template>
  <a-layout id="components-layout-demo-top-side-2">
    <a-layout>
      <a-layout-sider width="200" style="background: #fff">
        <side-menu :sideMenuList="sideMenuList" :selectedKeys="selectedKeys"></side-menu>
      </a-layout-sider>
      <a-layout style="padding: 0 24px 24px">
        <a-breadcrumb style="margin: 16px 0">
          <a-breadcrumb-item>兴趣点</a-breadcrumb-item>
          <a-breadcrumb-item>探索</a-breadcrumb-item>
        </a-breadcrumb>

        <a-card title="高评价兴趣点" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          <top-poi-table
            v-on:topTableChange="topTableChange"
            :poi="topBusiness"
            :count="topCount"
            :start="start"
            :total="total"
            :pagination="topPagination"
            :loading="topLoading"
          ></top-poi-table>
        </a-card>
        <br>

        <a-card title="分类兴趣点" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          <poi-select v-on:select="select"></poi-select>
          <br>
          <poi-explore-table
            v-on:select="select"
            :business="business"
            :count="count"
            :start="start"
            :total="total"
            :city="city"
            :state="state"
            :stars="stars"
            :reviewCount="reviewCount"
            :pagination="pagination"
            :loading="loading"
          ></poi-explore-table>
        </a-card>
      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script>
import SideMenu from '@/components/common/SideMenu'
import PoiSelect from './PoiSelect'
import PoiExploreTable from './PoiExploreTable'
import TopPoiTable from './TopPoiTable'
export default {
  components: {
    SideMenu,
    PoiSelect,
    PoiExploreTable,
    TopPoiTable
  },
  data () {
    return {
      sideMenuList: [
        { href: '/poi', name: '探索', type: 'user' },
        { href: '/poi/search', name: '搜索', type: 'laptop' },
        { href: '/poi/recommend', name: '推荐', type: 'notification' }
      ],
      selectedKeys: [0],
      selected: false,
      business: [],
      count: 0,
      start: 0,
      total: 0,
      city: '',
      state: '',
      stars: 0,
      reviewCount: '',
      pagination: {},
      loading: false,
      topBusiness: {},
      topCount: 0,
      topStart: 0,
      topTotal: 0,
      topPagination: {},
      topLoading: false
    }
  },
  methods: {
    select: function (data, city, state, stars, reviewCount) {
      console.log('select', data)
      this.business = data.businessDTOList
      this.count = data.count
      this.start = data.start
      this.total = data.total
      this.city = city
      this.state = state
      this.stars = stars
      this.reviewCount = reviewCount
      this.pagination = {
        current: this.start / this.count + 1,
        total: this.total,
        pageSize: this.count
      }
      this.loading = false
    },
    topTableChange: function (data) {
      console.log('topTableChange', data)
      this.topBusiness = data.businessDTOList
      this.topCount = data.count
      this.topStart = data.start
      this.topTotal = data.total
      this.topPagination = {
        current: this.topStart / this.topCount + 1,
        total: this.topTotal,
        pageSize: this.topCount
      }
      this.topLoading = false
    }
  },
  mounted () {
    console.log('mounted')
    this.$axios.get('/business/topBusiness', {
      params: {
        start: 0,
        count: 10
      }
    }).then(res => {
      console.log(res.data)
      var data = res.data.data
      this.topBusiness = data.businessDTOList
      this.topCount = data.count
      this.topStart = data.start
      this.topTotal = data.total
      this.topPagination = {
        current: this.topStart / this.topCount + 1,
        total: this.topTotal,
        pageSize: this.topCount
      }
    })
  }
}
</script>

<style scoped>
#components-layout-demo-top-side-2 .logo {
  width: 120px;
  height: 31px;
  background: rgba(255, 255, 255, 0.2);
  margin: 16px 28px 16px 0;
  float: left;
}
</style>
