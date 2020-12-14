<template>
  <a-layout id="components-layout-demo-top-side-2">
    <a-layout>
      <a-layout-sider width="200" style="background: #fff">
        <side-menu :sideMenuList="sideMenuList" :selectedKeys="selectedKeys"></side-menu>
      </a-layout-sider>
      <a-layout style="padding: 0 24px 24px">
        <a-breadcrumb style="margin: 16px 0">
          <a-breadcrumb-item>兴趣点</a-breadcrumb-item>
          <a-breadcrumb-item>推荐</a-breadcrumb-item>
        </a-breadcrumb>

        <a-card title="兴趣点推荐" :style="{ background: '#fff', padding: '24px', margin: 0, minHeight: '280px' }">
          <poi-recommend-select :algorithm="algorithm"></poi-recommend-select>
          <br>
          <form-tags :movieTitles="movieTitles"></form-tags>
          <br>
          <a-button type="primary" @click="submit">
            确定
          </a-button>
          <br>
          <poi-recommend-table
            v-on:recommendTableChange="recommendTableChange"
            :business="business"
            :count="count"
            :start="start"
            :total="total"
            :pagination="pagination"
            :loading="loading"
            :userId="userId"
            :algorithm="algorithm"
          ></poi-recommend-table>
        </a-card>

        <br>

      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script>
import SideMenu from '@/components/common/SideMenu'
import PoiRecommendSelect from './PoiRecommendSelect'
import FormTags from './FormTags'
import PoiRecommendTable from './PoiRecommendTable'
export default {
  components: {
    SideMenu,
    PoiRecommendSelect,
    FormTags,
    PoiRecommendTable
  },
  data () {
    return {
      sideMenuList: [
        { href: '/poi', name: '探索', type: 'user' },
        { href: '/poi/search', name: '搜索', type: 'laptop' },
        { href: '/poi/recommend', name: '推荐', type: 'notification' }
      ],
      selectedKeys: [2],
      userId: 'user_id',
      algorithm: 'Item2Vec',
      business: [],
      count: 0,
      start: 0,
      total: 0,
      pagination: {},
      loading: false
    }
  },
  methods: {
    submit () {
      console.log('submit', this.algorithm, this.movieTitles)
      const qs = require('qs')
      this.loading = true
      this.$axios.get('/business/recommend', {
        params: {
          algorithm: this.algorithm,
          userId: this.userId,
          start: 0,
          count: 10
        },
        // https://segmentfault.com/q/1010000010323643
        // 解决GET请求传数组的问题
        paramsSerializer: function (params) {
          return qs.stringify(params, {arrayFormat: 'repeat'})
        }
      }).then(res => {
        console.log(res.data)
        let data = res.data.data
        this.business = data.businessDTOList
        this.count = data.count
        this.start = data.start
        this.total = data.total
        this.pagination = {
          current: this.start / this.count + 1,
          total: this.total,
          pageSize: this.count
        }
        this.loading = false
      })
    },
    recommendTableChange (data) {
      console.log('recommendTableChange')
      this.business = data.businessDTOList
      this.count = data.count
      this.start = data.start
      this.total = data.total
      this.pagination = {
        current: this.start / this.count + 1,
        total: this.total,
        pageSize: this.count
      }
      this.loading = false
    }
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
