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
  </div>
</template>

<script>
export default {
  props: ['algorithm'],
  data () {
    return {
      value: 'Item2Vec'
    }
  },
  methods: {
    onChange (e) {
      console.log(`checked = ${e.target.value}`)
      this.algorithm = this.value
    }
  }
}
</script>
