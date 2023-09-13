<template>
  <div>
    <el-form label-position="top" label-width="90px">
      <el-form-item label="变量">
        <el-row v-for="(item, index) in config.variableList" :key="index">
          <el-col :span="10">
            <el-input size="mini" v-model="item.name" placeholder="请输入变量名" />
          </el-col>
          <el-col :span="10" :offset="1">
            <el-input size="mini" v-model="item.value" placeholder="请输入变量值" />
          </el-col>
          <el-col :span="2" :offset="1">
            <el-button size="mini" type="danger" icon="el-icon-delete" circle @click="handleDel(index)"></el-button>
          </el-col>
        </el-row>
        <el-button type="primary" size="mini" @click="handleVar">添加变量</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>

export default {
  name: "VariableNodeConfig",
  props: {
    config: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data() {
    return {
  
    }
  },
  computed: {
    nodeProps() {
      return this.$store.state.selectedNode.props
    }
  },
  methods: {
    handleVar() {
      this.config.variableList.push({
        name: '',
        value: undefined
      })
    },
    handleDel(index) {
      this.config.variableList.splice(index, 1)
    }
  }
}
</script>