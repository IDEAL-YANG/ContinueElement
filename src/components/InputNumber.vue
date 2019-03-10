<template>
  <div class="InputNumber">
    <h1>Input Number Trains</h1>
    <el-input-number v-model="num1" @change="handleChange" :min="1" :max="10" label="描述文字"></el-input-number>
    <hr>
    <el-input-number v-model="num2" @change="handleChange" :min="1" :max="10" label="描述文字" :step="2"></el-input-number>
    <hr>
    <el-input-number v-model="num3" @change="handleChange" :precision="3" :step="0.5"></el-input-number>
    <hr>
    <el-input-number v-model="num4" @change="handleChange" controls-position="right"></el-input-number>
  </div>
</template>

<script>
export default {
  name: 'InputNumber',
  data() {
    return {
      num1:1,
      num2:1,
      num3:10,
      num4:5
    };
  },
  methods: {
    handleChange(value) {
      console.log(value)
    }
  }
}
</script>

<style scoped>

</style>
