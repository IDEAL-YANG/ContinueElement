<template>
  <div>
    <el-slider v-model="value1"></el-slider>
    <hr>
    <el-slider v-model="value2"></el-slider>
    <hr>
    <el-slider v-model="value3" :show-tooltip="false"></el-slider>
    <hr>
    <el-slider v-model="value4" :format-tooltip="formatterTips"></el-slider>
    <hr>
    <el-slider v-model="value5" disabled></el-slider>
    <hr>
    <el-slider v-model="value6" :step="10"></el-slider>
    <hr>
    <el-slider v-model="value7" :step="10" :show-stops="true"></el-slider>
    <hr>
    <el-slider v-model="value8" :show-input="true"></el-slider>
    <hr>
    <el-slider v-model="value9" :step="10" :show-stops="true" range></el-slider>
    <hr>
    <el-slider v-model="value10" vertical height="200px"></el-slider>
  </div>
</template>

<script>
  export default {
    name: 'Slider',
    data () {
      return {
        value1: 0,
        value2: 50,
        value3: 35,
        value4: 66,
        value5: 87,
        value6: 10,
        value7: 90,
        value8: 10,
        value9: [40,80],
        value10: 30,
      };
    },
    methods: {
      formatterTips(val) {
        return val/100
      }
    }
  }
</script>

<style scoped>

</style>
