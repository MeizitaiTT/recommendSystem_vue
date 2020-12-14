<template>
  <div>
    <label for="tags-basic">请输入用户id按Enter键：</label>
    <b-form-tags
      input-id="tags-basic"
      v-model="movieTitles"
      placeholder=""
    ></b-form-tags>
    <!--    <p class="mt-2">Value: {{ movies }}</p>-->
  </div>
</template>

<script>
export default {
  props: {
    movieTitles: {
      default: ['user_id']
    }
  },
  data() {
    return {
    }
  }
}
</script>
