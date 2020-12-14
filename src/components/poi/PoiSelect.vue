<template>
  <div>
    请选择城市：
    <a-select default-value="---" style="width: 120px" @focus="handleCityFocus" @change="handleCityChange">
      <a-select-option v-for="(item, i) in cities" :key="i" :value="item" >
        {{item}}
      </a-select-option>
    </a-select>
    <br><br>

    请选择所在州：
    <a-select default-value="---" style="width: 120px" @focus="handleStateFocus" @change="handleStateChange">
      <a-select-option v-for="(item, i) in states" :key="i" :value="item">
        {{item}}
      </a-select-option>
    </a-select>
    <br><br>

    请选择评分星级：
    <a-select default-value="---" style="width: 120px" @change="handleStarChange">
      <a-select-option v-for="(item, i) in stars" :key="i" :value="item">
        {{item}}
      </a-select-option>
    </a-select>
    <br><br>

    请选择评价数量：
    <a-select default-value="---" style="width: 120px" @change="handleReviewCountChange">
      <a-select-option v-for="(item, i) in reviewCounts" :key="i" :value="item">
        {{item}}
      </a-select-option>
    </a-select>
    <br><br>

    <a-button type="primary" @click="submit">
      确定
    </a-button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      cities: [],
      states: [],
      city: '---',
      state: '---',
      star: 0,
      reviewCount: '---',
      // eslint-disable-next-line standard/array-bracket-even-spacing
      stars: ['---', '1', '2', '3', '4', '5' ],
      reviewCounts: ['---', '0 - 100', '100 - 400', '400 - 700', '700 - 1000', '1000以上']
    }
  },
  methods: {
    handleCityFocus () {
      if (this.cities.length <= 0) {
        this.$axios.get('/business/businessCity').then(res => {
          console.log(res.data)
          this.cities = res.data.data
          this.cities.unshift('---')
        })
      }
    },
    handleStateFocus () {
      if (this.states.length <= 0) {
        this.$axios.get('/business/businessState').then(res => {
          console.log(res.data)
          this.states = res.data.data
          this.states.unshift('---')
        })
      }
    },
    handleCityChange (value) {
      console.log(`selected ${value}`)
      this.city = value
    },
    handleStateChange (value) {
      console.log(`selected ${value}`)
      this.state = value
    },
    handleStarChange (value) {
      console.log(`selected ${value}`)
      this.star = value
    },
    handleReviewCountChange (value) {
      console.log(`selected ${value}`)
      this.reviewCount = value
    },
    submit () {
      console.log('submit', this.city, this.state, this.star, this.reviewCount)
      this.$axios.get('/business/condition', {
        params: {
          city: this.city,
          state: this.state,
          stars: this.star === '---' ? 0 : this.star,
          reviewCount: this.reviewCount === '' ? '---' : this.reviewCount,
          start: 0,
          count: 10
        }
      }).then(res => {
        console.log(res.data)
        this.$emit('select', res.data.data, this.city, this.state, this.star, this.reviewCount)
      })
    }
  }
}
</script>

<style scoped>

</style>
