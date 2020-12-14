<template>
  <a-input-search style="margin-bottom: 8px" placeholder="请输入兴趣点关键词" @search="onSearch" />
</template>

<script>
export default {
  methods: {
    onSearch (value) {
      console.log('onSearch', value)
      this.$emit('load')
      this.$axios.get('/business/search', {
        params: {
          keyword: value,
          start: 0,
          count: 10
        }
      }).then(res => {
        console.log(res.data)
        this.$emit('search', res.data.data, value)
      })
    }
  }
}
</script>
