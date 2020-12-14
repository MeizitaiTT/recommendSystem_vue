<template>
  <a-table
    :columns="columns"
    :data-source="data"
    :pagination="pagination"
    :loading="loading"
    @change="handleTableChange"
  >
    <span slot="state" slot-scope="state">
      <a-tag
        v-for="tag in state"
        :key="tag"
        color="geekblue"
      >
        {{ tag.toUpperCase() }}
      </a-tag>
    </span>
    <span slot="stars" slot-scope="stars">
      <a-tag
        :key="tag"
        color="green"
      >
      </a-tag>
    </span>
  </a-table>
</template>

<script>
const columns = [
  {
    title: '名字',
    dataIndex: 'name',
    key: 'name'
  },
  {
    title: '地址',
    dataIndex: 'fullAddress',
    key: 'fullAddress'
  },
  {
    title: '类型',
    dataIndex: 'categories',
    key: 'categories',
    width: '30%'
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
  props: ['poi', 'count', 'start', 'total', 'pagination', 'loading'],
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
        var poi = []
        for (let i = 0; i < this.poi.length; i++) {
          poi.push(this.poi[i])
          poi[i].key = i.toString()
        }
        console.log('getData', poi)
        return poi
      },
      set: function (newValue) {
      }
    }
  },
  methods: {
    handleTableChange (pagination, filters, sorter) {
      console.log(pagination)
      this.loading = true
      this.$axios.get('/business/topBusiness', {
        params: {
          start: (pagination.current - 1) * pagination.pageSize,
          count: pagination.pageSize
        }
      }).then(res => {
        console.log(res.data)
        this.$emit('topTableChange', res.data.data)
      })
    }
  }
}
</script>
