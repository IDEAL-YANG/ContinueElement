<template>
  <div>
    <el-select
      v-model="selectValue"
      @change="handleChangeValue"
      placeholder="哈啊哈"
      clearable>
      <el-option
        v-for="item in optionalList"
        :key="item.value"
        :label="item.label"
        :value="item.value"
        :disabled="item.disable"></el-option>
    </el-select>
    <hr>
    <el-select
      v-model="selectValues"
      @change="handleChangeValue"
      placeholder="哈啊哈" clearable multiple>
      <el-option
        v-for="item in optionalList"
        :key="item.value"
        :label="item.label"
        :value="item.value"
        :disabled="item.disable"></el-option>
    </el-select>

    <el-select style="margin-left: 20px"
               v-model="selectValues"
               @change="handleChangeValue"
               placeholder="哈啊哈"
               clearable
               multiple
               collapse-tags
               filterable>
      <el-option
        v-for="item in optionalList"
        :key="item.value"
        :label="item.label"
        :value="item.value"
        :disabled="item.disable"></el-option>
    </el-select>

    <hr>
    <el-select style="margin-left: 20px"
               v-model="selectValues"
               @change="handleChangeValue"
               placeholder="哈啊哈" clearable multiple>
      <el-option
        v-for="item in optionalList"
        :key="item.value"
        :label="item.label"
        :value="item.value"
        :disabled="item.disable">
        <label style="float: left">{{ item.label }}</label>
        <label style="float: right">{{ item.value }}</label>
      </el-option>
    </el-select>

    <hr>
    <el-select
      v-model="selectGroupValue"
      @change="handleChangeValue"
      placeholder="输入吧，骚年"
      clearable>
      <el-option-group
        v-for="group in groupOptionalList"
        :key="group.name"
        :label="group.name">
        <el-option
          v-for="item in group.dists"
          :key="item.value" :label="item.name" :value="item.desc"></el-option>
      </el-option-group>
    </el-select>

    <hr>
    <el-select v-model="selectValues"
               @change="handleChangeValue"
               placeholder="哈啊哈"
               clearable
               multiple
               collapse-tags
               filterable>
      <el-option
        v-for="item in optionalList"
        :key="item.value"
        :label="item.label"
        :value="item.value"
        :disabled="item.disable"></el-option>
    </el-select>

    <hr>
    <el-select
      v-model="value9"
      multiple
      filterable
      remote
      reserve-keyword="true"
      placeholder="输入输入"
      :remote-method="remoteMethod"
      :loading="loading">
      <el-option
        v-for="item in options4"
        :key="item.value"
        :label="item.label"
        :value="item.value"></el-option>

    </el-select>

    <hr>
    <el-select v-model="selectValues"
               multiple
               filterable
               allow-create
               default-first-option
               placeholder="请选择文章标签">
      <el-option v-for="item in optionalList"
                 :key="item.value"
                 :label="item.label"
                 :value="item.value">
      </el-option>
    </el-select>

  </div>
</template>

<script>
export default {
  name: 'Select',
  data () {
    return {
      optionalList: [
        { label: '中国大团结万岁', value: '1', disable: false },
        { label: '英国大团结万岁', value: '2', disable: false },
        { label: '意大利国大团结万岁', value: '3', disable: true },
        { label: '德国大团结万岁', value: '4', disable: false }
      ],
      selectValue: '',
      selectValues: [],
      groupOptionalList: [
        { name: '北京',
          desc: '欢迎你',
          dists: [
            {
              name: '通州区', desc: '通州人民'
            },
            {
              name: '昌平区', desc: '昌平人民'
            }
          ]
        },
        { name: '上海',
          desc: '不欢迎你',
          dists: [
            {
              name: '徐汇区', desc: '徐汇全体人民'
            },
            {
              name: '浦东新区', desc: '浦东万里人民'
            }
          ]
        }
      ],
      selectGroupValue: '',
      selectGroupValues: [],
      options4: [],
      value9: [],
      list: [],
      loading: false,
      states: ['Alabama', 'Alaska', 'Arizona',
        'Arkansas', 'California', 'Colorado',
        'Connecticut', 'Delaware', 'Florida',
        'Georgia', 'Hawaii', 'Idaho', 'Illinois',
        'Indiana', 'Iowa', 'Kansas', 'Kentucky',
        'Louisiana', 'Maine', 'Maryland',
        'Massachusetts', 'Michigan', 'Minnesota',
        'Mississippi', 'Missouri', 'Montana',
        'Nebraska', 'Nevada', 'New Hampshire',
        'New Jersey', 'New Mexico', 'New York',
        'North Carolina', 'North Dakota', 'Ohio',
        'Oklahoma', 'Oregon', 'Pennsylvania',
        'Rhode Island', 'South Carolina',
        'South Dakota', 'Tennessee', 'Texas',
        'Utah', 'Vermont', 'Virginia',
        'Washington', 'West Virginia', 'Wisconsin',
        'Wyoming']
    }
  },
  methods: {
    handleChangeValue (value) {
      console.log(value)
    },
    handleFilter (value) {

    },
    remoteMethod (query) {
      if (query !== '') {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.options4 = this.list.filter(item => {
            return item.label.toLowerCase().indexOf(query.toLowerCase()) > -1
          })
        }, 200)
      } else {
        this.options4 = []
      }
    }
  },
  mounted () {
    this.list = this.states.map(item => {
      return { value: item, label: item }
    })
  }
}
</script>

<style scoped>

</style>
