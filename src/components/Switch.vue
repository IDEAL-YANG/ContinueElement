<template>
  <div>
    <el-switch
      v-model="value1"
      active-color="#13ce66"
      inactive-color="#ff4949"></el-switch>
    <hr>
    <el-switch
      v-model="value2"
      active-text="年付"
      inactive-text="月付"></el-switch>
    <hr>
    <el-tooltip :content="'Switch value: ' + value3" placement="top">
      <el-switch
        v-model="value3"
        active-value="100"
        inactive-value="0"></el-switch>
    </el-tooltip>
    <hr>
    <el-switch
      v-model="value4" disabled></el-switch>
    <el-switch
      v-model="value5" disabled></el-switch>
  </div>
</template>

<script>
export default {
  name: 'Switch',
  data () {
    return {
      value1: true,
      value2: true,
      value3: true,
      value4: true,
      value5: false
    };
  }
}
</script>

<style scoped>

</style>
