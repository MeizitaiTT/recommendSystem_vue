<template>
  <a-table
    :columns="columns"
    :data-source="data"
    :pagination="pagination"
    :loading="loading"
    @change="handleTableChange"
  >
    <span slot="countries" slot-scope="countries">
      <a-tag
        v-for="tag in countries"
        :key="tag"
        color="geekblue"
      >
        {{ tag.toUpperCase() }}
      </a-tag>
    </span>
    <span slot="genres" slot-scope="genres">
      <a-tag
        v-for="tag in genres"
        :key="tag"
        color="green"
      >
        {{ tag.toUpperCase() }}
      </a-tag>
    </span>
  </a-table>
</template>

<script>
const columns = [
  {
    title: '名字',
    dataIndex: 'name',
    key: 'name',
    width: '20%'
  },
  {
    title: '地址',
    dataIndex: 'address',
    key: 'address',
    width: '20%'
  },
  {
    title: '类型',
    dataIndex: 'category',
    key: 'category'
  },
  {
    title: '城市',
    dataIndex: 'city',
    key: 'city'
  },
  {
    title: '所在州',
    dataIndex: 'state',
    key: 'state'
    // scopedSlots: { customRender: 'state' }
  },
  {
    title: '评分星级',
    dataIndex: 'stars',
    key: 'stars'
    // scopedSlots: { customRender: 'stars' }
  }
]

export default {
  props: ['business', 'count', 'start', 'total', 'city', 'state', 'stars', 'reviewCount', 'pagination', 'loading'],
  data () {
    return {
      // eslint-disable-next-line vue/no-dupe-keys
      loading: false,
      columns
    }
  },
  computed: {
    data: {
      get: function () {
        const business = []
        for (let i = 0; i < this.business.length; i++) {
          business.push(this.business[i])
          business[i].key = i.toString()
        }
        console.log('getData', business)
        return business
      },
      set: function (newValue) {
      }
    }
  },
  methods: {
    handleTableChange (pagination, filters, sorter) {
      console.log(pagination)
      this.loading = true
      this.$axios.get('/business/condition', {
        params: {
          city: this.city,
          state: this.state,
          stars: this.stars,
          reviewCount: this.reviewCount,
          start: (pagination.current - 1) * pagination.pageSize,
          count: pagination.pageSize
        }
      }).then(res => {
        console.log(res.data)
        this.$emit('select', res.data.data, this.city, this.state, this.stars, this.reviewCount)
      })
    }
  }
}
</script>
