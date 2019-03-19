<template>
    <div>
      <el-tree :data="data" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
      <hr>
      <el-tree :props="props" :load="loadNode" lazy show-checkbox @check-change="handleCheckChange"></el-tree>
      <hr>
      <el-tree :props="props1" :load="loadNode1" lazy show-checkbox></el-tree>
      <hr>
      <el-tree :props="defaultProps" :data="data2" show-checkbox default-expand-all node-key="id" ref="tree" highlight-current></el-tree>
      <el-button @click="getCheckedNodes">通过node获取</el-button>
      <el-button @click="getCheckeKeys">通过key获取</el-button>
      <el-button @click="setCheckedNodes">通过node设置</el-button>
      <el-button @click="setCheckedKeys">通过Key设置</el-button>
      <el-button @click="resetChecked">清空</el-button>

      <hr>
      <div class="custom-tree-container">
        <div class="block">
          <p>使用 render-content</p>
          <el-tree
            :data="data4"
            node-key="id"
            default-expand-all
            :expand-on-click-node="false"
            :render-content="renderContent">
          </el-tree>
        </div>
        <div class="block">
          <p>使用 scoped slot</p>
          <el-tree
            :data="data5"
            node-key="id"
            default-expand-all
            :expand-on-click-node="false">

            <div v-if="!node.isLeaf" class="custom-tree-node-header" slot-scope="{ node, data }">
              <div class="custom-tree-node">
                <div >
                <el-icon class="el-icon-setting"></el-icon>
                <el-button
                  size="mini"
                  type="text"
                  style="padding-left: 5px;"
                  v-if="!data.editing"
                  @click="() => switchEditing(data)"
                >{{ data.label }}</el-button>
                <input
                  v-focus
                  type="text"
                  style="padding-left: 5px;"
                  v-model="data.label"
                  v-else @blur="() => switchEditing(data)"/>
              </div>
                <div>
                <el-button
                  icon="el-icon-circle-plus-outline"
                  type="text"
                  size="mini"
                  @click="() => append(data)"></el-button>
                <el-button
                  icon="el-icon-delete"
                  type="text"
                  size="mini"
                  @click="() => remove(node, data)"></el-button>
              </div>
              </div>
            </div>

            <div v-else class="custom-tree-node-header" slot-scope="{ node, data }">

              <div class="custom-tree-node">
                <div >
                  <el-button v-if="data.showTags" size="mini" type="text" icon="el-icon-caret-bottom" @click="() => switchShowTags(data)"></el-button>
                  <el-button v-else size="mini" type="text" icon="el-icon-caret-right" @click="() => switchShowTags(data)"></el-button>
                  <el-icon class="el-icon-setting" style="padding-left: 5px;"></el-icon>
                  <el-button
                    size="mini"
                    type="text"
                    style="padding-left: 5px;"
                    v-if="!data.editing"
                    @click="() => switchEditing(data)"
                  >{{ data.label }}</el-button>
                  <input
                    v-focus
                    type="text"
                    style="padding-left: 5px;"
                    v-model="data.label"
                    v-else @blur="() => switchEditing(data)"/>
                </div>
                <div>
                <el-button
                  icon="el-icon-delete"
                  type="text"
                  size="mini"
                  @click="() => remove(node, data)"></el-button>
                </div>
              </div>

              <div style="height: auto; display: flex;justify-content: flex-start; flex-wrap: wrap;" v-if="node.isLeaf && data.showTags">
                <el-tag
                  v-for="(tag,index) in data.tags"
                  :key="index"
                  closable
                  @close="() => handleClose(index, data)"
                >{{ tag }}</el-tag>

                <input
                  class="input-new-tag"
                  v-focus
                  v-if="data.tagEditing"
                  v-model="inputTag"
                  size="small"
                  @keyup.enter="() => switchTagEditing(data)"
                  @blur="() => switchTagEditing(data)"
                />
                <el-button v-else class="button-new-tag" size="small" @click="() => switchTagEditing(data)">+ New Tag</el-button>
              </div>
            </div>
          </el-tree>
        </div>
        </div>

    </div>
</template>

<script>

let id = 1000

export default {
  name: 'Tree',
  data () {
    const data = [
      {
        id: 1,
        label: '一级 1',
        editing: false,
        isLeaf: false,
        showTags: false,
        tags: [],
        tagEditing: false,
        children: [{
          id: 4,
          label: '二级 1-1',
          editing: false,
          isLeaf: false,
          showTags: false,
          tags: [],
          tagEditing: false,
          children: [{
            id: 9,
            label: '三级 1-1-1',
            editing: false,
            showTags: true,
            tags: ['王嘉尔', '娜可露露'],
            tagEditing: false,
            isLeaf: true
          }, {
            id: 10,
            label: '三级 1-1-2',
            editing: false,
            showTags: true,
            tags: ['王嘉尔', '娜可露露'],
            tagEditing: false,
            isLeaf: true
          }]
        }]
      },
      {
        id: 2,
        label: '一级 2',
        editing: false,
        isLeaf: false,
        showTags: false,
        tags: [],
        tagEditing: false,
        children: [{
          id: 5,
          editing: false,
          label: '二级 2-1',
          showTags: true,
          tags: ['王嘉尔', '娜可露露'],
          tagEditing: false,
          isLeaf: true
        }, {
          id: 6,
          editing: false,
          label: '二级 2-2',
          showTags: true,
          tags: ['王嘉尔', '娜可露露'],
          tagEditing: false,
          isLeaf: true
        }]
      },
      {
        id: 3,
        editing: false,
        label: '一级 3',
        isLeaf: false,
        showTags: false,
        tags: [],
        tagEditing: false,
        children: [{
          id: 7,
          editing: false,
          label: '二级 3-1',
          showTags: true,
          tags: ['王嘉尔', '娜可露露'],
          tagEditing: false,
          isLeaf: true
        }, {
          id: 8,
          editing: false,
          label: '二级 3-2',
          showTags: true,
          tags: ['王嘉尔', '娜可露露'],
          tagEditing: false,
          isLeaf: true
        }]
      }
    ]

    return {
      data: [
        {
          label: '一级 1',
          children: [
            {
              label: '二级 1-1',
              children: [
                {
                  label: '三级 1-1-1'
                }
              ]
            }
          ]
        },
        {
          label: '一级 2',
          children: [
            {
              label: '二级 2-1',
              children: [
                {
                  label: '三级 2-1-1'
                }
              ]
            }
          ]
        },
        {
          label: '一级 3',
          children: [
            {
              label: '二级 3-1',
              children: [
                {
                  label: '三级 3-1-1'
                }
              ]
            },
            {
              label: '二级 3-2',
              children: [
                {
                  label: '三级 3-2-1'
                }
              ]
            }
          ]
        }
      ],
      defaultProps: {
        children: 'children',
        label: 'label'
      },
      props: {
        label: 'name',
        children: 'zones'
      },
      count: 1,
      props1: {
        label: 'name',
        children: 'zones',
        isLeaf: 'leaf'
      },
      data2: [
        {
          id: 1,
          label: '一级 1',
          children: [{
            id: 4,
            label: '二级 1-1',
            children: [{
              id: 9,
              label: '三级 1-1-1'
            }, {
              id: 10,
              label: '三级 1-1-2'
            }]
          }]
        },
        {
          id: 2,
          label: '一级 2',
          children: [{
            id: 5,
            label: '二级 2-1'
          }, {
            id: 6,
            label: '二级 2-2'
          }]
        },
        {
          id: 3,
          label: '一级 3',
          children: [{
            id: 7,
            label: '二级 3-1'
          }, {
            id: 8,
            label: '二级 3-2'
          }]
        }
      ],
      data4: JSON.parse(JSON.stringify(data)),
      data5: JSON.parse(JSON.stringify(data)),
      inputTag: ''
    }
  },
  methods: {
    handleNodeClick (data) {
      console.log(data)
    },
    handleCheckChange (data, checked, indeterminate) {
      console.log(data, checked, indeterminate)
    },
    loadNode (node, resolve) {
      if (node.level === 0) {
        return resolve([{ name: 'region1' }, { name: 'region2' }])
      }
      if (node.level > 3) return resolve([])

      var hasChild
      if (node.data.name === 'region1') {
        hasChild = true
      } else if (node.data.name === 'region2') {
        hasChild = false
      } else {
        hasChild = Math.random() > 0.5
      }
      setTimeout(() => {
        var data
        if (hasChild) {
          data = [
            {
              name: 'zone' + this.count++
            }, {
              name: 'zone' + this.count++
            }
          ]
        } else {
          data = []
        }

        resolve(data)
      }, 500)
    },
    loadNode1 (node, resolve) {
      if (node.level === 0) {
        return resolve([{ name: 'region' }])
      }
      if (node.level > 1) return resolve([])

      setTimeout(() => {
        const data = [{
          name: 'leaf',
          leaf: true
        }, {
          name: 'zone'
        }]

        resolve(data)
      }, 500)
    },
    getCheckedNodes () {
      console.log(this.$refs.tree.getCheckedNodes())
    },
    getCheckeKeys () {
      console.log(this.$refs.tree.getCheckedKeys())
    },
    setCheckedNodes () {
      this.$refs.tree.setCheckedNodes([{
        id: 5,
        label: '二级 2-1'
      }, {
        id: 9,
        label: '三级 1-1-1'
      }])
    },
    setCheckedKeys () {
      this.$refs.tree.setCheckedKeys([3])
    },
    resetChecked () {
      this.$refs.tree.setCheckedKeys([])
    },

    switchEditing (data) {
      data.editing = !data.editing
    },
    switchShowTags (data) {
      data.showTags = !data.showTags
    },
    handleClose (index, data) {
      data.tags.splice(index, 1)
    },

    switchTagEditing (data) {
      console.log(data)
      data.tagEditing = !data.tagEditing
      if (data.tagEditing === false && this.inputTag.length > 0) {
        data.tags.push(this.inputTag)
      }
      this.inputTag = ''
    },

    append (data) {
      const newChild = { id: id++, label: 'testtest', editing: false, isLeaf: false, showTags: true, children: [] }
      if (!data.children) {
        this.$set(data, 'children', [])
      }
      data.children.push(newChild)
    },

    remove (node, data) {
      const parent = node.parent
      const children = parent.data.children || parent.data
      const index = children.findIndex(d => d.id === data.id)
      children.splice(index, 1)
    },

    renderContent (h, { node, data, store }) {
      return (
        <span class="custom-tree-node">
          <span><el-button class="el-icon-tickets" size="mini" type="text">{ node.label }</el-button></span>
          <span>
            <el-button size="mini" type="text" on-click={ () => this.append(data) }>Append</el-button>
            <el-button size="mini" type="text" on-click={ () => this.remove(node, data) }>Delete</el-button>
          </span>
        </span>)
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<style>
  .custom-tree-node-header {
    flex: 1;
  }
  .custom-tree-node {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 14px;
    padding-right: 8px;
  }
  .el-tree-node__content {
    height: auto;
  }
  .el-tag + .el-tag {
    margin-left: 10px;
    margin-bottom: 10px;
  }
  .button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
  }
  .input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
  }
</style>
