<template>
  <div>
    请选择推荐算法：
    <a-radio-group button-style="solid" v-model="value" @change="onChange">
      <a-radio-button value="ItemCF">
        ItemCF
      </a-radio-button>
      <a-radio-button value="JIM">
        JIM
      </a-radio-button>
      <a-radio-button value="STSTM">
        STSTM
      </a-radio-button>
      <a-radio-button value="USTTM">
        USTTM
      </a-radio-button>
      <a-radio-button value="SPORE">
        SPORE
      </a-radio-button>
    </a-radio-group>
    <br />
    请选择出行类型：
    <a-radio-group :options="plainOptions" :default-value="value1" @change="onChange1" />
    <br><br>
    如异地请选择出行所在州：
    <a-select default-value="---" style="width: 120px" @focus="handleStateFocus" @change="handleStateChange">
      <a-select-option v-for="(item, i) in states" :key="i" :value="item">
        {{item}}
      </a-select-option>
    </a-select>
    <br><br>
    选择出行时间：
    <a-date-picker @change="onTimeChange" v-model="valueDate"
      format="YYYY-MM-DD HH:mm"
      :show-time="{ defaultValue: moment('00:00', 'HH:mm') }"
    />
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import moment from 'moment'
// eslint-disable-next-line no-unused-vars
const plainOptions = ['同城', '异地']
// eslint-disable-next-line no-unused-vars
const options = [
  { label: '同城', value: 'currentCity' },
  { label: '异地', value: 'anotherCity' }
]
export default {
  props: ['algorithm'],
  data () {
    return {
      value: 'Item2Vec',
      plainOptions,
      options,
      state: '---',
      states: [],
      visitDate: '',
      valueDate: ''
    }
  },
  methods: {
    moment,
    onChange (e) {
      console.log(`checked = ${e.target.value}`)
      this.algorithm = this.value
    },
    onTimeChange (val, dateString) {
      this.visitDate = val
      console.log(this.visitDate, dateString)
    },
    onChange1 (e) {
      console.log('radio1 checked', e.target.value)
    },
    handleStateFocus () {
      // eslint-disable-next-line eqeqeq
      if (this.states.length <= 0) {
        this.$axios.get('/business/businessState').then(res => {
          console.log(res.data)
          this.states = res.data.data
          this.states.unshift('---')
        })
      }
    }
  }
}

</script>
